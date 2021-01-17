---
title: "Flutter Part 2: with Dart"
date: 2019-11-22 14:14:58
hidden: false
draft: false
tags: ["flutter","App"]
slug: "Flutter 2"
---
## Instructon of Dart

Get basic idea of both flutter and dart:
 - [Flutter with Dart](https://medium.com/swlh/flutter-with-dart-is-it-worth-it-3f60ed5df9da)
 - [Flutter for web developers](https://flutter.dev/docs/get-started/flutter-for/web-devs)

<!--more-->

### var by `Type`

Dart build-in types:

  * *String* variableSting
  * *int* variableNumber
  * *double* variableNumberWithFloat
  * *bool* booleanType
  * *List<Class>* listVariableFromClass
  * etc...

```
String myName;              // myName = 'Esther'
int number;                 // number = 1
double numberFloat;         // numberFloat = 4.12
bool isHot;                 // isHot = true
List<int> numberArray;      // numberArray = [1, 2, 3, ...]
List<String> stringArray;   // stringArray = ['a', 'b', 'c', ...]
```

See all build-in types on [Dart doc](https://dart.dev/guides/language/language-tour#built-in-types).

📌 Know more about [Generics in Dart](https://dart.dev/guides/language/language-tour#generics)

### Pokers by Dart

 - All dart need `main()`
 - Object-oriented with **Class**


```
void main() {
  var deck = new Deck();
  deck.shuffle();
  print(deck.deal(5));
}

class Deck {
  List<Card> cards = [];

  Deck() {
    var suits = ['Daimonds', 'Hearts', 'Clubs', 'Spades'];
    var ranks = ['Ace', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K'];

    for (var s in suits) {
      for (var r in ranks) {
        var card = new Card(
          rank: r,
          suit: s,
        );
        cards.add(card);
      }
    }
  }

  toString() {
    return cards.toString();
  }

  shuffle() {
    cards.shuffle();
  }

  cardsWithSuit(String suit) {
    return cards.where((card) => card.suit == suit);
  }

  deal(int handSize) {
    var hand = cards.sublist(0, handSize);
    cards = cards.sublist(handSize);
    return hand;
  }

  removeCard(String suit, String rank) {
    cards.removeWhere((card) => (card.suit == suit) && (card.rank == rank));
  }
}

class Card {
  String suit;
  String rank;

  Card({this.rank, this.suit});

  toString() {
    return '$rank of $suit';
  }
}

```