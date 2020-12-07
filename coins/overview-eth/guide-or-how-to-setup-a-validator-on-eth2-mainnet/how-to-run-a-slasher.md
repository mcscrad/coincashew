---
description: >-
  Running slasher is completely optional and yet can help protect the chain and
  earn you extra ETH.
---

# How to run a slasher

## ⏩ Quick steps guide

{% hint style="info" %}
The following steps align with our [mainnet guide](./). You may need to adjust file names and directory locations where appropriate. The core concepts remain the same.
{% endhint %}

### 🗡 What is a Slasher?

* Validators might commit slashable transactions such as
  * **Double Voting**: signs more than one block in same epoch
  * **Surround Votes**: signs two contradictory attestations
* A slasher will submit proof of the above offenses into a block proposal.
* Running a slashing helps protect and keeps the network healthy.
* If your slasher successfully detects an offense and is lucky enough to include proof of the offense into your scheduled block proposal, your validator earns extra ETH.

{% hint style="warning" %}
Slashers tend to be resource intensive and is currently recommended for advanced users only.
{% endhint %}

### 🤖 Minimum Slasher System Requirements

* Quad-core or more CPU
* 16+ GB RAM
* 256GB+ SSD HD

### 🚧 How to Configure the Slasher

Slasher functionality is currently available in Lighthouse or Prysm.

{% tabs %}
{% tab title="Lighthouse " %}
```bash
# Edit your beacon-chain unit file
sudo nano /etc/systemd/system/beacon-chain.service

# Append the following slasher flag to ExecStart
--slasher --debug-level debug

# Example of what your ExecStart should look like.
# lighthouse bn --staking --metrics --network mainnet --slasher --debug-level debug

# Reload the new unit file
sudo systemctl daemon-reload

# Restart your beacon-chain
sudo systemctl restart beacon-chain
```
{% endtab %}

{% tab title="Prysm" %}
```bash
# Create a new slasher unit file
cat > $HOME/slasher.service << EOF 
# The eth2 slasher service (part of systemd)
# file: /etc/systemd/system/slasher.service 

[Unit]
Description     = eth2 slasher service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/prysm/prysm.sh slasher --mainnet
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF

# Move the unit file to /etc/systemd/system 
sudo mv $HOME/slasher.service /etc/systemd/system/slasher.service

# Update file permissions.
sudo chmod 644 /etc/systemd/system/slasher.service

# Enable auto-start at boot time and then start your slasher service.
sudo systemctl daemon-reload
sudo systemctl enable slasher
sudo systemctl start slasher
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Nice work. You're running a slasher now. 🗡 🤖 🔪 
{% endhint %}

{% hint style="info" %}
Be sure to familiarize yourself with the [reference material for detailed official slasher documentation.](how-to-run-a-slasher.md#reference-material)
{% endhint %}

##  🤖 Start staking by building a validator <a id="start-staking-by-building-a-validator"></a>

### Visit here for our [Mainnet guide](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet) and here for our [Testnet guide](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-testnet). <a id="visit-here-for-our-mainnet-guide-and-here-for-our-testnet-guide"></a>

{% hint style="success" %}
Congrats on completing the guide. ✨

Did you find our guide useful? Send us a signal with a tip and we'll keep updating it.

It really energizes us to keep creating the best crypto guides.

Use [cointr.ee to find our donation](https://cointr.ee/coincashew) addresses. 🙏

Any feedback and all pull requests much appreciated. 🌛

Hang out and chat with fellow stakers on Discord @

​[https://discord.gg/w8Bx8W2HPW](https://discord.gg/w8Bx8W2HPW) 😃
{% endhint %}

![](../../../.gitbook/assets/gg.jpg)

\*\*\*\*🎊 **2020-12 Update**: We're on [Gitcoin](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew), where you can contribute via [quadratic funding](https://vitalik.ca/general/2019/12/07/quadratic.html) and make a big impact.  Your **1 DAI** contribution equals a **28 DAI** match. 

Please [check us out](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew). Thank you!🙏

{% embed url="https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew" %}

## 🧩 Reference Material

{% embed url="https://docs.prylabs.network/docs/prysm-usage/slasher/" %}

{% embed url="https://lighthouse-book.sigmaprime.io/slasher.html" %}

