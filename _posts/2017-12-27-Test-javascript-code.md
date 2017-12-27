---
layout: post
title: Test javascript code
tags: [javascript, tdd]
fullview: true
---

Nice test in javascript

{% highlight javascript %}
    describe("Tic tac toe draw test"() => {
        const board = givenBoard()
            .atGivenState([['X', 'X', 'O']
                           ['O', 'X', 'X']
                           [' ', 'O', 'O']])
            .create()
        const game = givenGame()
            .withBoard(board)
            .whereNextTurnIsX()
            .create()

        game = game.makeMove(2, 0)

        assertEqual(game.result, 'DRAW')
    })
{% endhighlight %}


Super cool!
