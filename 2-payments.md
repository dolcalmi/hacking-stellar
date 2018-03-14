*Links:*
[Front](https://github.com/0xfe/hacking-stellar/blob/master/README.md) -
[Chapter 1](https://github.com/0xfe/hacking-stellar/blob/master/1-launch.md) -
[Chapter 2](https://github.com/0xfe/hacking-stellar/blob/master/2-payments.md)

# Chapter 2. Payments

Payments are the *raison-d'etre* of Stellar. No t

## Working with the Lumen tool

Now that we have two accounts: `bob` and `kelly`, lets try to execute some payments between them. To do this, we'll use Lumen's `pay` command. Note that you can always find out more about a command's usage and parameters with the `--help` flag.

```sh
$ lumen --help
Lumen is a commandline client for the Stellar blockchain

Usage:
  lumen [flags]
  lumen [command]

Available Commands:
  account     manage stellar keypairs and accounts
  asset       manage stellar assets
  balance     check the balance of [asset] on [account]
  del         delete variable
  dex         trade assets on the DEX
  friendbot   fund [address] on the test network with friendbot
  get         get variable
  help        Help about any command
  ns          set namespace to [namespace]
  pay         send [amount] of [asset] from [source] to [target]
  set         set variable
  signer      manage signers on account
  trust       manage trustlines between accounts and assets
  version     get version of lumen CLI
  watch       watch the account on the ledger

Flags:
  -h, --help             help for lumen
      --network string   network to use (test) (default "test")
      --ns string        namespace to use (default) (default "default")
      --store string     namespace to use (default) (default "file:/Users/mo/.lumen-data.yml")
  -v, --verbose          verbose output (false)

Use "lumen [command] --help" for more information about a command.
```

Let's see what `lumen pay` lets us do.

```sh
 $ lumen pay --help
send [amount] of [asset] from [source] to [target]

Usage:
  lumen pay [amount] [asset] --from [source] --to [target] [flags]

Flags:
      --from string           source account seed or name
      --fund                  fund a new account
  -h, --help                  help for pay
      --max string            spend no more than this much during path payments
      --memoid string         memo ID
      --memotext string       memo text
      --path stringSlice      comma-separated list of paths, uses auto pathfinder if empty
      --signers stringSlice   alternate signers (comma separated)
      --to string             target account address or name
      --with string           make a path payment with this asset

Global Flags:
      --network string   network to use (test) (default "test")
      --ns string        namespace to use (default) (default "default")
      --store string     namespace to use (default) (default "file:/Users/mo/.lumen-data.yml")
  -v, --verbose          verbose output (false)
```

Before we proceed, lets make sure we're using the test network.

```sh
$ lumen set config:network test
```

Kelly pays Bob 5 XLM for the cookie she just bought, and leaves a thank you message with the `--memotext` flag.

```sh
$ lumen pay 5 --from kelly --to bob --memotext 'thanks for the cookie'
$ lumen balance kelly
$ lumen balance bob
```

Every transaction can have a short memo associated (upto 28 bytes long.) The memo can used by a payment processor or business to redirect funds. For example, *Mary's Bank* can have a single stellar account for all its customers, and require payers to fill in the memo to designate the recipient. Instead of a text memo, the bank could require a numeric one, for which we use the `--memoid` flag.

```sh
$ lumen account set MarysBank GAFUU44WASFPD4YHIU5TKVMGLFMOHAQIOUJTTFQ432W65PFYQVVXFSUW
$ lumen pay 5 --from kelly --to MarysBank --memoid 485532245
```

Notice that we created an account alias for Mary's Bank called `MarysBank`. Aliases make it easier to work with Stellar, since you don't have to keep pasting in long addresses in your commands. Lumen keeps track of these aliases in a state file in your home directory. Lumen also recognizes if an alias is an address or seed and uses them contextually depending on the command.

For example, in the above `pay` command, Lumen used Kelly's seed instead of her address (because the seed is what actually *signs* and authorizes the transaction.) If we didn't have Kelly's seed, the transaction would fail. Lets see what happens if you try to make a payment from Mary's Bank.

```sh
$ lumen pay 10 --from MarysBank --to kelly --memotext 'i am h4xor'
# Output: error
```

Obviously, we don't have their private seed -- and hopefully we never will.

*Links:*
[Front](https://github.com/0xfe/hacking-stellar/blob/master/README.md) -
[Chapter 1](https://github.com/0xfe/hacking-stellar/blob/master/1-launch.md) -
[Chapter 2](https://github.com/0xfe/hacking-stellar/blob/master/2-payments.md)