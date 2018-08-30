# Fork AmityCoin

A guide on how to create your own cryptocurrency using AmityCoin as a codebase. We used [TurtleCoin](https://github.com/turtlecoin/turtlecoin-docs/wiki/Forking-TurtleCoin)'s forking guide as a guide.

#### Licenses

When you start to look into AmityCoin, you will stumble across this;

```
// Copyright (c) 2012-2017, The CryptoNote developers, The Bytecoin developers
// Copyright (c) 2018, The Amity Developers
```

The first copyright represents the CryptoNote codebase which AmityCoin is initially based on, then you will see the AmityCoin copyright for our edited/forked codebase.
If you fork AmityCoin, you need to make sure you add a new line of copyright in the header to state that this is now your forked codebase like so;

```
// Copyright (c) 2012-2017, The CryptoNote developers, The Bytecoin developers
// Copyright (c) 2018, The Amity Developers
// Copyright (c) 2018, The AmityCoin Community Developers
```

#### Make It Yours

The main file you will need to be editing to make a fork of AmityCoin is `CryptoNoteConfig.h` as this hold's the main editing parameter's. These are only the constant's that need to be changed as the rest can be left as they are. This can be found in `amitycoin/src/`

Open up `CryptoNoteConfig.h` in a text editor, I use NotePad++ myself and follow the forking instruction's for this file;

`const uint64_t DIFFICULTY_TARGET = 90; // seconds` 

This is how long it should take to solve a block. AmityCoin solve's a block every minute and a half. It's measure is seconds so if you want a 3 minute block time you would set it as 3 x 60 `= 180;`

`const uint64_t CRYPTONOTE_PUBLIC_ADDRESS_BASE58_PREFIX = 0x1bf3c9;`

This is the address prefix constant. If you've got an AmityCoin wallet, you will notice that it starts with `amit`. This is because we have the value set to `0x1bf3c9`.  If you head over to the CryptoNote site, you can [use this tool](https://cryptonotestarter.org/tools.html) to find a suitable address prefix for your new coin.

`const uint64_t MONEY_SUPPLY = UINT64_C(100000000000000);`

Atomic Unit's are used in the codebase as decimal's would break it. The `MONEY_SUPPLY` is calculated like this;

If you have 2 decimal point's for you currency and you want a supply of 1 million, this equal's to 100 million in Atomic Unit's as 2 extra 0's are added to include the decimal place's. In AmityCoin, we have a `MONEY_SUPPLY` of 10 billion. This mean's our Atomic Unit equal's to 1 trillion because we have 4 decimal place's. *Aren't they cool Atomic Unit's?*

`const unsigned EMISSION_SPEED_FACTOR = 25;`

This control's how your coin is distributed when you come to mining. We used [ForkNote's Tool](http://forknote.net/create/#/) to calculate the correct factor exactly.

`const size_t   CRYPTONOTE_REWARD_BLOCKS_WINDOW = 298.01896;`

This isn't a fixed amount but give's a rough idea although this is AmityCoin's exact block reward. When you calculate your emission factor, fill this constant with the updated block reward amount given from the site.

`const size_t   CRYPTONOTE_DISPLAY_DECIMAL_POINT = 4;`

Decide the amount of decimal place's you want then update this constant. We use 4 to make it easier keeping track of your AMIT balance.

`const uint64_t MINIMUM_FEE = UINT64_C(10);`

Control's the fee when sending money, this is measured in Atomic Unit's. We have it set at `10` so if you sent a 1 AMIT you would have to have 1.0010 in your balance.

`const uint64_t DEFAULT_DUST_THRESHOLD = MINIMUM_FEE;`

Is how much you can send to somebody else. You can 0.0010 to a friend as this goes of the rate of the fee.

`const char     CRYPTONOTE_NAME[] = "Amitycoin";`

This will be the name of new cryptocurrency.

`const char     GENESIS_COINBASE_TX_HEX[] = "";`

After you compile your coin, load your daemon with `--print-genesis-tx` flag then paste in to here the new tx

```
const int      P2P_DEFAULT_PORT = 37000;
const int      RPC_DEFAULT_PORT = 37001;
```

P2P is your daemon connecting port and RPC is daemon to outsider's. The RPC port is used when connecting to simplewallet or anyother outside services. Do a quick google search for uncommon port's as some are already claimed by `chrome` `edge` `and so on...` default program's of your OS. If your port clashe's, it will not connect.

```
const std::initializer_list<const char*> SEED_NODES = {
  //"your_seed_ip1.com:37000",
  //"your_seed_ip2.com:37000",
};
```

This is where your seed node's go. Remember you have to have 2 node's online for an active network but is recommended 5 *just in case*. Follow the syntax and use P2P port's

```
const std::initializer_list<CheckpointData> CHECKPOINTS = {
  //{ 10000, "84b6345731e2702cdaadc6ce5e5238c4ca5ecf48e3447136b2ed829b8a95f3ad" },
};
```

This is not one of our checkpoint's. Listing checkpoint's in this constant help's keep your network on track. 

`const uint64_t GENESIS_BLOCK_REWARD = UINT64_C(0);`

You'll notice that this constant isn't in our codebase for the simple fact we did not have a premine. You may add this to your codebase if you really want. It's measured in Atomic Unit's.
You will need to create a wallet with simplewallet before you open up your daemon. Create a wallet and copy the address. Open the daemon with `--print-genesis-tx --genesis-block-reward-address <paste that address here>`
Add the new tx to the codebase and you're good to go.

Next you will need to open up `amitycoin/src/P2p/P2pNetworks.h` in a text editor and edit the network value's to a unique one like so;

```
{
  const static boost::uuids::uuid CRYPTONOTE_NETWORK = { { 0x4F, 0x52, 0x2A, 0x30, 0x5B, 0x4C, 0x2E, 0x4A, 0x59, 0x45, 0x00, 0x75, 0x1D, 0xF1, 0xA1, 0xA3 } };
}
```

Number's 0 to 9 and letter's A to F can be inserted here to change your network.

##### When You've Done That

You should be ready to go. Make sure you added at least 2 new IP's as seed node's with the P2P port following it to make your network work. Add your new genesis tx. Load up you're Linux Terminal, I use a subsystem on Windows with Ubuntu, then follow below;

```
$ sudo git clone https://github.com/your-amity-fork-git
$ cd into/that/forkname
$ sudo mkdir build && cd build
$ sudo cmake .. && sudo make -j4
```

Wait until it reache's 100% and wala, you've fork AmityCoin!

#### So What Next?

Head over to our Discord server and make the announement to our team! Then it's up to you to maintain your coin. Good Luck Guy's!
