# Lab 07

## Q1: WWPD: Linked Lists

`python3 ok -q link -u --local`

答案：[link.py](https://github.com/weijiew/cs61a/blob/master/lab/lab07/tests/link.py)


## Q2: Making Cards

很简单！

```python
    def __init__(self, name, attack, defense):
        self.name = name
        self.attack = attack
        self.defense = defense
```

`python3 ok -q Card.__init__ --local`

```python
    def power(self, other_card):
        return self.attack - other_card.defense / 2
```

`python3 ok -q Card.power --local`

## Q3: Making a Player


```python
class Player:
    def __init__(self, deck, name):
        self.deck = deck
        self.name = name
        "*** YOUR CODE HERE ***"
        self.hand = []
        for i in range(5):
            self.hand.append(deck.draw())

    def draw(self):
        assert not self.deck.is_empty(), 'Deck is empty!'
        "*** YOUR CODE HERE ***"
        self.hand.append(self.deck.draw())
    
    def play(self, card_index):
        "*** YOUR CODE HERE ***"
        return self.hand.pop(card_index)
```

## Q4: Link to List


```python
def link_to_list(link):
    res  = []
    while link:
        res.append(link.first)
        link = link.rest
    return res
```

## Q5: Cumulative Mul

```python
def cumulative_mul(t):
    if t.is_leaf():
        pass
    else:
        for b in t.branches:
            cumulative_mul(b)
            t.label *= b.label
```

