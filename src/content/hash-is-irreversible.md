---
layout: post
title: 'Is hashing really a irreversible process?'
author: [Dan]
tags: ['hash']
image: img/welcome-to-ghost.jpg
date: '2021-05-10'
draft: false
excerpt: 해시는 정말 역계산을 할 수 없을까?
---

> [https://stackoverflow.com/questions/47017606/is-hashing-really-a-irreversible-process](https://stackoverflow.com/questions/47017606/is-hashing-really-a-irreversible-process)

암호화폐와 관련된 영상을 보다가 해시에 대한 내용이 나오길래 이런 질문이 생겨서 찾아보니 관련 글을 찾았다.

# Question

I've been using hash and RSA for a time(on a very superficial level, for ex: RSA authentication on a SSH connection), and I want to learn more about it.

To begin with, I know that encryption is a two-way process that can be reverted. And hashing is a one-way process that is irreversible.

That last point just doesn't make sense to me, if I use an algorithm to hash "hello", wouldn't the same algorithm, but "reversed"(meaning, it works "backwards"), be able to convert that hash to "hello" again.

암호화는 양방향성을 가진다.

암호화와 복호화가 가능하다. 하지만 해시는 그렇지 않다.

# Answer

It is irreversible in the sense that for each input you have exactly one output, but not the other way around. There are multiple inputs that yields the same output.

For any given input, there's a lot (infinite in fact) different inputs that would yield the same hash. This is easy to realize since the output has a fixed size, but the input doesn't have size restrictions.

To achieve this, irreversible math is used. For instance, it is easy to calculate `10%3`. The answer to that is simply `10%3=1`. But if I give you the equation `x%3=1`, what would you do? This equation is true for all `x=3*k+1`. Thus, you cannot get the number I started with.

> 여기까지만 읽어도 안된다는걸 알 수 있지만 계속 읽어보자.

Another example of irreversible math is sine and cosine. For instance, `cos(0)=1`, but there are more input values that evaluates to 1. Actually, `cos(n*2pi)=1`. There are "inverses" to these functions, but they either gives an answer in a certain range or a multivalued answer.

A third example is `x²=1`. This is true for both `x=1` and `x=-1`. However, in this example you get a finite (and also rather small) number of possible answers.

When dealing with encryption, one could say that the private key is used to pick the right solution. You can always quite trivially decrypt an encrypted message, but you will get a huge load of possible answers. The key is used to find the right one, rather than actually decrypting.

Another thing worth mentioning is that a good hash algorithm `minimizes for collisions`, that is, two inputs generating the same hash.

Also, when it comes to cryptography, you want it to be as hard as possible to reverse it. But this also comes with the cost that algorithms are cpu intensive.

A very basic and insecure hash algorithm could look like this pseudocode:

```bash
hash = 0
for each byte in input:
    hash = hash + byte

```

Here I assume that `hash` is a simple integer variable that wraps around when it comes to it's maximum value. The algorithm is easy to implement, and it's quick.

But you don't want to use it if security is important. It's typically very easy to modify a file so that this hash check won't detect it.

Real cryptographic hash algorithms strives to achieve the property that if you change **any** single bit in the input, **every** single bit in the output have a 50% chance of flipping. Furthermore, if you flip two bits in the input, the bits flipped in the output will be totally unrelated to which bits would flip if you just changed the bits one by one.

I found a good youtube video on the subject: [https://www.youtube.com/watch?v=yoMOAIzBSpY](https://www.youtube.com/watch?v=yoMOAIzBSpY)

해시 충돌이란 해시값을 만들었을 때 다른 digest가 같은 해시값을 나타내는 것이다.

여러 예시를 들어주면서 친절하게 설명해주었는데, 요지는 가능한 값들을 만들어 낼 수 있지만, 그것이 해시값이라고 할 수는 없다.

또한 암호학에서 사용되는 해시는 2개의 비트만 바뀌어도 50%의 해시값이 변경되는 구조로 되어 있다. (충돌 가능성이 낮다는 이야기는 포함되어 있지 않지만, 충돌 가능성도 낮을 것이라고 생각된다.)
