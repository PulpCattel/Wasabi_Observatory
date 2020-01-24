## Don't Trust, Verify!

### Get the data

The Bitcoin Blockchain is openly verifiable, anyone without the need for any kind of permission can look at it.

A simple way that requires nothing more than an Internet connection is to query some API. 

Blockstream explorer, for example, offers a nice set of API that you can use over Tor with an onion service (recommended).

*Example API request over onion service*: http://explorerzydxu5ecjrkwceayqybizmpjjznk5izmitf2modhcusuqlid.onion/api/address/bc1qa24tsgchvuxsaccp8vrnkfd85hrcpafg20kmjw

Or if you don't want to use Tor.

*Example API request over https*: https://blockstream.info/api/address/bc1qa24tsgchvuxsaccp8vrnkfd85hrcpafg20kmjw

*Blockstream API documentation*: https://github.com/Blockstream/esplora/blob/master/API.md

There are many different Blockchain explorers that offer API, pick the one that works best for you.

Another way of getting the data is to use your own full node. 

*Useful links*: 
* https://github.com/jgarzik/python-bitcoinrpc            
* https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_calls_list
* https://en.bitcoin.it/wiki/API_reference_(JSON-RPC)               
* https://github.com/nopara73/WasabiVsSamourai
* https://github.com/jgmontoya/wasabi-cj-stats

### Analyze the data

Once you have the data you are interested in, you can work on it using your favorite programming language. 
Here there is the complete list of stats used and their exact calculation. If you find any error, or something doesn't add up, please let me know so I can fix it.

* [CoinJoin per day](Dont_Trust_Verify.md#coinjoin-per-day)
* [Partecipants per CoinJoin](Dont_Trust_Verify.md#partecipants-per-coinjoin)
* [Average input size per CoinJoin](Dont_Trust_Verify.md#average-input-size-per-coinjoin)
* [Total volume](Dont_Trust_Verify.md#total-volume)
* [Total number of addresses](Dont_Trust_Verify.md#total-number-of-addresses)
* [Percentage remixers per CoinJoin](Dont_Trust_Verify.md#percentage-remixers-per-coinjoin)
* [Total percentage remixers](Dont_Trust_Verify.md#total-percentage-remixers)
* [Percentage address reuse per CoinJoin](Dont_Trust_Verify.md#percentage-address-reuse-per-coinjoin)
* [Total percentage address reuse](Dont_Trust_Verify.md#total-percentage-address-reuse)
* [Total number equal outputs](Dont_Trust_Verify.md#total-number-equal-outputs)
* [Percentage equal outputs reused per CoinJoin](Dont_Trust_Verify.md#percentage-equal-outputs-reused-per-coinjoin)
* [Total percentage equal outputs reused](Dont_Trust_Verify.md#total-percentage-equal-outputs-reused)
* [Coordinator fees per CoinJoin](Dont_Trust_Verify.md#coordinator-fees-per-coinjoin)
* [Total coordinator fees](Dont_Trust_Verify.md#total-coordinator-fees)

---

* #### CoinJoin per day

**The green line** is the number of CoinJoin per day.

**The blue line** is the average number of CoinJoin per day.

* #### Partecipants per CoinJoin

**The green dots** are the distribution of the number of partecipants per each CoinJoin.

**The blu line** is the average number of partecipants.

* #### Average input size per CoinJoin

**The green dots** are the distribution of the average input sizes for each CoinJoin.

`Average input size per CoinJoin = sum(CoinJoin inputs) / number(CoinJoin inputs)`

**The blue line** is the average of the distribution.

Example:

##### CoinJoin round 1

```
inputs > amounts

A > 1
B > 2
C > 3
D > 4
E > 5
```

`average per CoinJoin = (1+2+3+4+5)/5 = 3`

`average overall = 3/1 = 3`

##### CoinJoin round 2

```
inputs > amounts

F > 2
G > 5
H > 6
I > 8
J > 1
```

`average per CoinJoin = (2+5+6+8+1) / 5 = 4.4`

`average overall = (3+4.4)/2 = 3.7`

* #### Total volume

**The green line** is the total input volume.

**The blue line** is the total equal output volume.

Example:

##### CoinJoin round 1

```
inputs > amounts |  | amounts 

A > 1 |  | 1
B > 2 |  | 1
C > 3 |  | 1
D > 4 |  | 1
E > 5 |  | 1
         | 2
         | 2
         | change
         | change
         | ...
```

`total input volume = 1+2+3+4+5 = 15`

`total mixed volume = 1+1+1+1+1+2+2 = 9`

* #### Total number of addresses

**The green line** is the total number of addresses that partecipate as input in a CoinJoin.

**The blue line** is the total number of inputs. 

> Ideally, green and blue should be equal, each input should be a separate address. In practice people reuse addresses multiple times.

**The light blue line** is the total number of remixers. An input address is counted as a remixer if it spends a previous equal value output and if it appears as input not more than once and as equal value output not more than once (either in the same CoinJoin or in different ones).

**The orange line** is the total number of addresses reused. An input address is counted as reused if it appears as input in a CoinJoin more than once (either in the same CoinJoin or in different ones).

**The red line** is the total number of inputs that belong to reused addresses. 

Example:

##### CoinJoin round 1

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > Z
B > 2 |  | 1 > Y
C > 3 |  | 1 > X
         | change
         | ...
```

`total number addresses = A,B,C = 3`

`total number inputs = A,B,C = 3`

`total number remixers = 0`

`total number addresses reused = 0`

`total number inputs reused = 0`

##### CoinJoin round 2

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > W
Z > 1 |  | 1 > V
E > 3 |  | 1 > U
         | change
         | ...
```

`total number addresses = A,B,C,Z,E = 5`

`total number inputs = A,B,C,A,Z,E = 6`

`total number remixers = Z = 1`

`total number addresses reused = A = 1`

`total number inputs reused = A,A = 2`

* #### Percentage remixers per CoinJoin

**The green dots** are the distribution of the remixers percentage for each CoinJoin. An input address is counted as a remixer if it spends a previous equal value output and if it appears as input not more than once and as equal value output not more than once (either in the same CoinJoin or in different ones).

`remixers percentage per CoinJoin = number(CoinJoin remixers) / number(CoinJoin inputs)`

**The blue line** is the average of the distribution.

Example:

##### CoinJoin round 1

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > Z
B > 2 |  | 1 > Y
C > 3 |  | 1 > X
         | change
         | ...
```

`remixers percentage per CoinJoin = 0%`

`average percentage = 0%`

##### CoinJoin round 2

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > W
Z > 1 |  | 1 > V
E > 3 |  | 1 > U
         | change
         | ...
```
`remixers percentage per CoinJoin = 1/3 = 33%`

`average percentage = (0+33)/2 = 16.5%`

* #### Total percentage remixers

**The green line** is the percentage of remixers on the total number of addresses that partecipated as input in a CoinJoin. An input address is counted as a remixer if it spends a previous equal value output and if it appears as input not more than once and as equal value output not more than once (either in the same CoinJoin or in different ones).

`percentage addresses = number(total remixers) / number(total input addresses)`

**The blue line** is the percentage of remixers on the total number of inputs.

`percentage inputs = number(total remixers) / number(total inputs)`

Example:

##### CoinJoin round 1

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > Z
B > 2 |  | 1 > Y
C > 3 |  | 1 > X
         | change
         | ...
```

`percentage addresses = 0%`

`percentage inputs = 0%`

##### CoinJoin round 2

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > W
Z > 1 |  | 1 > V
E > 3 |  | 1 > U
         | change
         | ...
```
`percentage addresses = (Z)/(A,B,C,Z,E) = 1/5 = 20%`

`percentage inputs = (Z)/(A,B,C,A,Z,E) = 1/6 = 16%`

* #### Percentage address reuse per CoinJoin

**The orange dots** are the distribution of the percentage of addresses reused for each CoinJoin. An input address is counted as reused if it appears as input in a CoinJoin more than once (either in the same CoinJoin or in different ones).

`percentage addresses reuse per CoinJoin = number(CoinJoin addresses reused) / number(CoinJoin input addresses)`

**The red dots** are the distribution of the percentage of inputs that belong to reused addresses.

`percentage inputs reused per CoinJoin = number(CoinJoin inputs reused) / number(CoinJoin inputs)`

**The red and orange lines** are the average of the respective distribution.

Example:

##### CoinJoin round 1

```
inputs 

A,A,B,B,C,D,E
```

`n° addresses reused = A,B = 2`

`n° inputs reused = A,A,B,B = 4`

`tot n° addresses = A,B,C,D,E = 5`

`tot n° inputs = A,A,B,B,C,D,E = 7`

`percentage addresses reuse per CoinJoin = n° addresses reused / n° addresses = 2/5 = 40%`

`percentage inputs reused per CoinJoin = n° inputs reused / n° inputs = 4/7 = 57%`

`average % address reuse per CoinJoin = 40/1 = 40%`

`average % inputs reused per CoinJoin = 57/1 = 57%`

##### CoinJoin round 2

```
inputs

F,C,G,H,I,I
```

`n° addresses reused = C,I = 2`

`n° inputs reused = C,I,I = 3`

`tot n° addresses = F,C,G,H,I = 5`

`tot n° inputs = F,C,G,H,I,I = 6`

`percentage addresses reuse per CoinJoin = n° addresses reused / n° addresses = 2/5 = 40%`

`percentage addresses reuse per CoinJoin = n° inputs reused / n° inputs = 3/6 = 50%`

`average % address reuse per CoinJoin = (40+40)/2 = 40%`

`average % inputs reused per CoinJoin = (57+50)/2 = 53.5%`

* #### Total percentage address reuse

**The orange line** is the percentage of address reuse on the total number of addresses that partecipated as input in a CoinJoin. An input address is counted as reused if it appears as input in a CoinJoin more than once (either in the same CoinJoin or in different ones).

`percentage addresses reused = number(total addresses reused) / number(total input addresses)`

**The red line** is the percentage of inputs that belong to reused addresses on the total number of inputs.

`percentage inputs reused = number(total inputs reused) / number(total inputs)`

Example:

##### CoinJoin round 1

```
inputs 

A,A,B,B,C,D,E
```

`n° addresses reused = A,B = 2`

`n° inputs reused = A,A,B,B = 4`

`tot n° addresses = A,B,C,D,E = 5`

`tot n° inputs = A,A,B,B,C,D,E = 7`

`percentage addresses reused = n° addresses reused / tot n° addresses = 2/5 = 40%`

`percentage inputs reused = n° inputs reused / tot n° inputs = 4/7 = 57%`

##### CoinJoin round 2

```
inputs

F,C,G,H,I,I
```

`n° addresses reused = A,B,C,I = 4`

`n° inputs reused = A,A,B,B,C,C,I,I = 8`

`tot n° addresses = A,B,C,D,E,F,G,H,I = 9`

`tot n° inputs = A,A,B,B,C,D,E,F,C,G,H,I,I = 13`

`percentage addresses reused = n° addresses reused / tot n° addresses = 4/9 = 44%`

`percentage inputs reused = n° inputs reused / tot n° inputs = 8/13 = 61%`

* #### Total number equal outputs

**The green line** is the total number of equal value outputs addresses.

**The blue line** is the total number of equal value outputs.

> Ideally, green and blue should be the same, each equal value output should be a separate address. In practice some addresses are reused multiple times. 
> This is either intentional or a bug (more [here](https://github.com/zkSNACKs/WalletWasabi/issues/2034) and [here](https://github.com/zkSNACKs/WalletWasabi/issues/2077)).

**The orange line** is the total number of equal value outputs addresses reused. An equal value output address is counted as reused if it appears as equal value output in a CoinJoin more than once (either in the same CoinJoin or in different ones).

**The red line** is the total number of equal value outputs that belong to equal value outputs addresses reused.

* #### Percentage equal outputs reused per CoinJoin

**The red dots** are the distribution of the percentage of equal value outputs that belong to output addresses reused for each CoinJoin. An equal value output address is counted as reused if it appears as equal value output in a CoinJoin more than once (either in the same CoinJoin or in different ones).

`percentage equal value outputs reused per CoinJoin = number(CoinJoin equal outputs reused) / number(CoinJoin equal outputs)`

**The red line** is the average of the distribution.

Example:

##### CoinJoin round 1

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > Z
B > 2 |  | 1 > Y
C > 3 |  | 1 > X
         | change
         | ...
```

`percentage outputs reused per CoinJoin = 0%`

`average percentage = 0%`

##### CoinJoin round 2

```
inputs > amounts |  | amounts > outputs

D > 1 |  | 1 > Z
E > 1 |  | 1 > W
F > 3 |  | 1 > V
         | change
         | ...
```

`percentage outputs reused per CoinJoin = (Z)/(Z,W,V) = 1/3 = 33%`

`average percentage = (0+33)/2 = 16.5%`

* #### Total percentage equal outputs reused

**The orange line** is the percentage of equal value outputs addresses reused on the total number of equal value outputs addresses. An equal value output address is counted as reused if it appears as equal value output in a CoinJoin more than once (either in the same CoinJoin or in different ones).

**The red line** is the percentage of equal value outputs that belong to equal value outputs addresses reused on the total number of equal value outputs. 

Example:

##### CoinJoin round 1

```
inputs > amounts |  | amounts > outputs

A > 1 |  | 1 > Z
B > 2 |  | 1 > Y
C > 3 |  | 1 > X
         | change
         | ...
```

`percentage equal value outputs addresses reused = 0%`

`percentage equal value outputs reused = 0%`

##### CoinJoin round 2

```
inputs > amounts |  | amounts > outputs

D > 1 |  | 1 > Z
E > 1 |  | 1 > W
F > 3 |  | 1 > V
         | change
         | ...
```

`percentage equal value outputs addresses reused = (Z)/(Z,Y,X,W,V) = 1/5 = 20%`

`percentage equal value outputs reused = (Z,Z)/(Z,Y,X,Z,W,V) = 2/6 = 33%`

* #### Coordinator fees per CoinJoin

**The green dots** are the distribution of the coordinator fees for each CoinJoin, the fees are calculate based on the values of the Coordinator address outputs.

**The blue line** is the average of the distribution.

* #### Total coordinator fees

**The green line** is the comulative sum of the coordinator fees, the fees are calculate based on the values of the Coordinator address outputs.
