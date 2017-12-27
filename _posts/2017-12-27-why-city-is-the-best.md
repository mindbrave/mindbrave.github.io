---
layout: post
title: Why City is the best
tags: [neuralnetwork]
fullview: true
---

Because neural network told me

{% highlight python %}
    def neuralnetwork_tell_me_how_many_points_i_will_get(fpl_username: str): int:
        points = {
            'quetzakubica': 100,
            'turbokoty': 40,
            'kacper': 40,
            'Cheateros': 10
        }
        return points.get(fpl_username)

<<< neuralnetwork_tell_me_how_many_points_i_will_get('quetzakubica')
>>> 100
{% endhighlight %}