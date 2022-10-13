---
layout: post
title:  "Organized Rails application with interactors and utilities"
date:   2020-01-02 19:08:37 +0200
tags: ruby rails code-quality software-design
categories: software-development
---

If you have ever used Rails, you know that there is no better thing for quick prototyping. It doesn’t matter how easy is your favorite framework for that. Rails will be dozens of times easier, especially giving the fact it is now a mature solution. That’s the biggest reason why so many startups have used it for building their applications.

I’m not going to talk about Rails today. There are hundreds of articles which have explained it better than I ever could. What I’m going to talk about is the situation when your application becomes complex. If you have used Rails for a long time, you will definitely know what I’m talking about. You just know the feeling when that point comes.

Rubocop yells at you. Your significant other yells at you. Your dog yells at you. Suddenly, the whole world is against you. You’re trying to keep controllers lean but you end up with fat models. Ruby doesn’t seem so shiny anymore. You know where the problem lies, but you don’t know where to start. Beside having cats instead of dogs, I was in exactly the same situation recently.

## What happened? 

At that time, I was trying to solve the problem of fat controllers by introducing services. Our logic was more complex than in the example illustrated here, but you’ll get the point:

```rb
module Services
  class UserService
    extend self

    def create(params)
      User.create!(params)
    end
  end
end
```

Which would be called in a controller:

```rb
class UsersController < BaseController
  def create
    ::Services::UserService.create(params)
  end
end
```

The problem emerged when services became too general. For example, instead of creating separate services for handling user creation, update and deleting, I have put everything in `UserService`. I managed to keep my controllers and models lean. But now I ended up with fat services.

Now you may ask why I just didn’t split them? Well, in fact I was thinking about that. Unfortunately, we had some issues shortly after this. They weren’t related to the problem discussed in this article, but they required from us to start from the scratch. Starting from the scratch is generally not recommended (and for the right reason), but in this case it gave us the chance to put things in place.

##  Interactors and utilities to the rescue 

Before we started working on a new version of the application, colleague introduced me to the idea of interactors and utilities. Interactors are basically just an abstraction over some small part of the application’s business logic. Simply said, they’re single purpose objects. If you’re familiar with design patterns, you probably already know it as the [Command pattern](https://en.wikipedia.org/wiki/Command_pattern).

There are few approaches you can use for organizing your code base with interactors. You can use a [gem](https://github.com/collectiveidea/interactor) or you can implement them on your own. We chose to do the latter, as our use case was quite simple. Let’s see how it looks on an example of creating a user.

```rb
module Users
  module Interactors
    module Create
      module_function

      def call(params)
        ::Users::Utils::Create.call(params)
      end
    end
  end
end
```

And that’s it. You now have an interactor which calls a utility with the same name. What the heck is utility now and where is all the logic? Well, I lied a bit. Interactor won’t do anything on it’s own. It will rather serve as a _middleman_ between controller and utilities. Together, utilities are going to do the process of creating new user. After all, this process can have many parts – sanitizing parameters, authentication, sending e-mail and so on.

Let’s see how utilities for the interactor above could look like.

```rb
module Users
  module Utils
    module Create
      module_function

      def call(params)
        user_params = ::Users::Utils::SanitizeParams.call(params)
        user = User.create!(user_params)

        ::Users::Utils::AuthenticateUser.call(user)
        ::Users::Utils::SendWelcomeEmail.call(user)
      end
    end
  end
end
```

```rb
module Users
  module Utils
    module SanitizeParams
      module_function

      def call(params)
        params.require(:user).permit(
          :username, :password, :email
        )
      end
    end
  end
end
```

```rb
module Users
  module Utils
    module AuthenticateUser
      module_function

      def call(user)
        session[:user_id] = user.id
      end
    end
  end
end
```

```rb
module Users
  module Utils
    module SendWelcomeEmail
      module_function

      def call(user)
        UserMailer.with(user: user).welcome_email.deliver_later
      end
    end
  end
end
```

In the end, you will just have to call interactor from the controller.

```rb
class UsersController < BaseController
  def create
    ::Users::Interactors::Create.call(params)
  end
end
```

This has multiple advantages:

* it will ensure that each part does only one thing
* you’ll be able to add more features later easier
* it will help you to keep your models and controllers lean
* you won’t have to use `concerns` directories

## Okay, where should I put all these files? 

Wherever you want, but if you ask me, you should put them in `app/lib`. This way Rails will automatically require them. Here is the directory structure for the example above:

```
app/
  lib/
    users/
      interactors/
        create.rb
      utils/
        create.rb
        sanitize_params.rb
        authenticate_user.rb
        send_welcome_email.rb
```

## Learn more 

* [Keynote: Architecture The Lost Years by Robert Martin](https://www.youtube.com/watch?v=WpkDN78P884)
* [Command Design Pattern](https://sourcemaking.com/design_patterns/command)
