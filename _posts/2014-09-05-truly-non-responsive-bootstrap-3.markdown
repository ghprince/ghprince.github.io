---
layout: post
title:  "Truly Non-Responsive Bootstrap 3"
date:   2014-09-05 03:56:47
---

Today I tried to make one of my Bootstrap 3 (v3.2.0) based website non-responsive because it is a pure desktop web app. I started with the [Official Guide][1] but it has several problems.

- Navbar. `.navbar-right` will disappear in a smaller window. (try resize the window and scroll to right)
- Inline Form. In a smaller window, `.form-control` under `form-inline` will expand to quite long input box and push elements after it to next line.

That's NOT non-responsive to me.

Then I found the `non-responsive.css` file in the source of [Offical Example][2]. So I loaded it into my app, and it fixed the Navbar issue (by a whole bunch of css lines). But the Inline Form issue still existed.

I then posted the question to [Stackoverflow][3]. The answer people provided worked great, but again, a whole bunch of css lines.

I am not a front-end engineer (but full-stack maybe?), so I hate those confusing css. Then I had an idea, **there should be a way to cheat the whole Bootstrap framework to make it believe that the user is viewing in a large screen**.

So after several hours(yep, I am not good at this) of digging the source and trial-and-error , I found the way I think is the best and cleanest to make bootstrap REALLY non-responsive. And now I am trying to mark down my final solution so that the world won't lose another several hours of productivity.

I am using `sass-rails` gem, so I prefer override to modification.

In `application.css.scss` (Rails app assumed), add these lines:

{% highlight scss %}
$screen-sm-min: 0px;
@import "bootstrap";

body {
  min-width: $screen-desktop;
}

.container {
  width: $container-md !important;
}
{% endhighlight %}

Note:

1. `$screen-sm-min: 0px;` must be added before `@import "bootstrap"` so that Bootstrap can pick up the value and set related ones accordingly.

2. `min-width` of `body` is required to fix a background color issue of Navbar.

3. `width` of `.container` is required as stated in [Official Guide][1]

[1]: http://getbootstrap.com/getting-started/#disable-responsive
[2]: http://getbootstrap.com/examples/non-responsive/
[3]: http://stackoverflow.com/questions/25672349/non-responsive-inline-form-in-bootstrap-3
