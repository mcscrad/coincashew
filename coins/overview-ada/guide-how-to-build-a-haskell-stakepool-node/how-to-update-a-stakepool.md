# How to update a Stakepool

## 🎉 ∞ Pre-Announcements

{% hint style="info" %}
Thank you for your support and kind messages! It really energizes us to keep creating the best crypto guides. Use [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. 🙏 
{% endhint %}

{% hint style="success" %}
As of Dec 9 2020, this guide is written for **mainnet** with **release v.1.24.2** 😁 
{% endhint %}

## 📡 1. How to perform an update

From time to time, there will be new versions of `cardano-node`. Follow the [Official Cardano-Node Github Repo](https://github.com/input-output-hk/cardano-node) by enabling **notifications** with the watch functionality.

To update with `$HOME/git/cardano-node` as the current binaries directory, copy the whole cardano-node directory to a new place so that you have a backup.

```bash
cd $HOME/git
rm -rf cardano-node-old/
git clone https://github.com/input-output-hk/cardano-node.git cardano-node2
cd cardano-node2/
```

{% hint style="danger" %}
Read the patch notes for any other special updates or dependencies that may be required for the latest release.
{% endhint %}

{% tabs %}
{% tab title="v1.24.2 Notes" %}
This release provides support for the upcoming Allegra and Mary hard forks and the new features they bring.

* The Allegra hard fork adds some features needed to support the Catalyst treasury scheme. It extends the existing multi-sig script language with predicates for time, via the slot number. It allows, for example, to make a script address that is not spendable until a certain point in time.
* The Mary hard fork adds multi-asset support. This is comparable to ERC20 and ERC721 tokens, but supported natively in the UTxO ledger. This is part of the Goguen feature set. It is a very significant feature and will have implications for all Cardano wallet implementations, including exchanges.

Stake Pool Operators \(SPOs\) and Exchanges should update their node config \( `mainnet-config.json`\) "options" section with an extra entry:

```text
  "options": {
    "mapBackends": {
      "cardano.node.resources": [
        "EKGViewBK"
      ],
```
{% endtab %}

{% tab title="v1.23.0 Notes" %}
This release includes a substantial amount of internal changes to support the upcoming Allegra and Mary hard forks and the new features they bring. This is _not_ the final release before the Allegra hard fork, but it does include the bulk of the functionality for both Allegra and Mary hard forks.

* The Allegra hard fork adds some features needed to support the Catalyst treasury scheme. It extends the existing multi-sig script language with predicates for time, via the slot number. It allows, for example, to make an address not spendable until a certain point in time.
* The Mary hard fork adds multi-asset support. This is comparable to ERC20 and ERC721 tokens, but supported natively in the UTxO ledger. This is part of the Goguen feature set. It is a very significant feature and will have implications for all Cardano wallet implementations, including exchanges.

Another notable change in this release is an adjustment to the pool ranking that will benefit small pools that have not yet made many blocks. We have adjusted the initial Bayesian prior so that instead of assuming new pools will perform at some less-than-perfect average level, we assume they will perform more-or-less perfectly. This prior is still updated based on the actual performance history, so pools that perform poorly will still drop in ranking. This change will especially benefit small pools that have produce few blocks so far, because they have very little performance history and so their score will be more influenced by the initial prior.

### Release v1.23.0 New Dependencies

Install GHC version 8.10.2

```bash
cd
wget https://downloads.haskell.org/ghc/8.10.2/ghc-8.10.2-x86_64-deb9-linux.tar.xz
tar -xf ghc-8.10.2-x86_64-deb9-linux.tar.xz
rm ghc-8.10.2-x86_64-deb9-linux.tar.xz
cd ghc-8.10.2
./configure
sudo make install
```

Update cabal and ensure GHC version 8.10.2 is installed.

```bash
source $HOME/.bashrc
cabal update
ghc -V
```

> \#Example of version output
>
> The Glorious Glasgow Haskell Compilation System, version 8.10.2

#### Disable Liveview

As if this release, LiveView was removed.

Run the following to modify **mainnet-config.json** and 

* change LiveView to SimpleView

```bash
cd $NODE_HOME
sed -i mainnet-config.json \
    -e "s/LiveView/SimpleView/g"
```

#### Install gLiveView

A recommended drop in replacement for LiveView to help you monitor activities on the node.

Refer to [this link for install instructions.](./#18-13-gliveview-node-status-monitoring)

#### Update permissions on vrf.skey \(only for block producer node\)

{% hint style="info" %}
Starting with version 1.23.0, `vrf.skey` permission checking has been implemented and a block producer node will only start if the owner is set to read-only permission.
{% endhint %}

```bash
cd $NODE_HOME
chmod 400 vrf.skey
```
{% endtab %}
{% endtabs %}

### Compiling the new binaries

Remove the old binaries and rebuild the latest binaries. Run the following command to pull and build the latest binaries. Change the checkout **tag** or **branch** as needed.

```bash
cd $HOME/git/cardano-node2
cabal clean
cabal update
rm -rf $HOME/git/cardano-node2/dist-newstyle/build/x86_64-linux/ghc-8.6.5
rm -rf $HOME/git/cardano-node2/dist-newstyle/build/x86_64-linux/ghc-8.10.2
git clean -fd
git fetch --all --recurse-submodules --tags
git checkout tags/1.24.2 && git pull
cabal configure -O0 -w ghc-8.10.2
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
```

{% hint style="info" %}
Build process may take a few minutes up to a few hours depending on your computer's processing power.
{% endhint %}

Verify your **cardano-cli** and **cardano-node** were updated to the expected version.

```bash
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") version
```

{% hint style="danger" %}
Stop your node before updating the binaries.
{% endhint %}

{% tabs %}
{% tab title="systemd" %}
```
sudo systemctl stop cardano-node
```
{% endtab %}

{% tab title="block producer node" %}
```bash
killall -s 2 cardano-node
```
{% endtab %}

{% tab title="relaynode1" %}
```
killall -s 2 cardano-node
```
{% endtab %}
{% endtabs %}

Copy **cardano-cli** and **cardano-node** files into bin directory.

```bash
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
```

```bash
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

Verify your **cardano-cli** and **cardano-node** were copied successfully and updated to the expected version.

```bash
cardano-node version
cardano-cli version
```

{% hint style="success" %}
Now restart your node to use the updated binaries.
{% endhint %}

{% tabs %}
{% tab title="systemd" %}
```
sudo systemctl start cardano-node
```
{% endtab %}

{% tab title="block producer node" %}
```bash
cd $NODE_HOME
./startBlockProducingNode.sh
```
{% endtab %}

{% tab title="relaynode1" %}
```
cd $NODE_HOME
./startRelayNode1.sh
```
{% endtab %}
{% endtabs %}

Finally, the following sequence will switch-over to your newly built cardano-node folder while keeping the old directory for backup.

```bash
cd $HOME/git
mv cardano-node/ cardano-node-old/
mv cardano-node2/ cardano-node/
```

{% hint style="danger" %}
**Reminder**: Don't forget to update your **air-gapped offline machine \(cold environment\)** with the new Cardano binaries.
{% endhint %}

{% hint style="success" %}
Congrats on completing the update. ✨ 

Did you find our guide useful? Send us a signal with a tip and we'll keep updating it. 

It really energizes us to keep creating the best crypto guides. 

Use [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. 🙏 

Any feedback and all pull requests much appreciated. 🌛 

Hang out and chat with fellow stake pool operators on Discord @

[https://discord.gg/w8Bx8W2HPW](https://discord.gg/w8Bx8W2HPW) 😃 

Hang out and chat with our stake pool community on Telegram @ [https://t.me/coincashew](https://t.me/coincashew)
{% endhint %}

## 🤯 2. In case of problems

### 🛣 4.1 Forked off

Forget to update your node and now your node is stuck on an old chain?

Reset your database files and be sure to grab the [latest genesis, config, topology json files](https://hydra.iohk.io/job/Cardano/cardano-node/cardano-deployment/latest-finished/download/1/index.html).

```bash
cd $NODE_HOME
rm -rf db
```

### 📂 4.2 Roll back to previous version from backup

{% hint style="danger" %}
Stop your node before updating the binaries.
{% endhint %}

{% tabs %}
{% tab title="block producer node" %}
```bash
killall -s 2 cardano-node
```
{% endtab %}

{% tab title="relaynode1" %}
```
killall -s 2 cardano-node
```
{% endtab %}

{% tab title="systemd" %}
```
sudo systemctl stop cardano-node
```
{% endtab %}
{% endtabs %}

Restore the old repository.

```bash
cd $HOME/git
mv cardano-node/ cardano-node-rolled-back/
mv cardano-node-old/ cardano-node/
```

Copy the binaries to `/usr/local/bin`

```bash
sudo cp $(find $HOME/git/cardano-node/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

Verify the binaries are the correct version.

```bash
/usr/local/bin/cardano-cli version
/usr/local/bin/cardano-node version
```

{% hint style="success" %}
Now restart your node to use the updated binaries.
{% endhint %}

{% tabs %}
{% tab title="block producer node" %}
```bash
cd $NODE_HOME
./startBlockProducingNode.sh
```
{% endtab %}

{% tab title="relaynode1" %}
```bash
cd $NODE_HOME
./startRelayNode1.sh
```
{% endtab %}

{% tab title="systemd" %}
```
sudo systemctl start cardano-node
```
{% endtab %}
{% endtabs %}

### 🤖 4.3 Last resort: Rebuild from source code

Follow the steps in [How to build a Stakepool.](./)

