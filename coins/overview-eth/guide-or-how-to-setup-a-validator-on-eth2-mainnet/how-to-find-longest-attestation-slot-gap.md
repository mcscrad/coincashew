---
description: Find the best slot gap to avoid missing any validator duties.
---

# How to find longest attestation slot gap

## ⏩ Quick steps guide

{% hint style="info" %}
The following steps align with our [mainnet guide](./). You may need to adjust file names and directory locations where appropriate. The core concepts remain the same.
{% endhint %}

### 🗡 Why do I want to calculate the longest attestation slot gap?

* Your validators are assigned duties to attest and propose blocks.
* Understanding the schedule of your validator's duties better, you can find the best time to plan eth1/eth2 updates, system reboots or outages.

### 🤖 Pre-requisites

* python3
* validator index number\(s\) -- Lookup on [https://beaconcha.in/](https://beaconcha.in/) or [https://beaconscan.com/](https://beaconscan.com/)
* Works with **Lighthouse / Teku / Prysm** currently

{% hint style="success" %}
✨ Kudos to **pietjepuk2** on Discord for authoring this process.
{% endhint %}

### 🚧 How to Run the Validator Duties script

1. Install python3

```text
sudo apt update && sudo apt-get install python3
```

2. Download [pietjepuk2](https://gist.github.com/pietjepuk2)'s `get_validator_duties.py` python script.

{% tabs %}
{% tab title="Lighthouse \| Teku" %}
```bash
cd $HOME
wget https://gist.githubusercontent.com/pietjepuk2/eb021db978ad20bfd94dce485be63150/raw/cc874b3035f97495416353f203d70477b31ab05d/get_validator_duties.py
```
{% endtab %}

{% tab title="Prysm" %}
```bash
cd $HOME
# modified by mohamedmansour for prysm
wget https://gist.githubusercontent.com/mohamedmansour/9a82071802ffd58bef7ab5db530f23fd/raw/d48a3f0948cf2ae8cf571b42d50f80d66841118f/get_validator_duties.py
```
{% endtab %}
{% endtabs %}

3. Enter your validator index numbers as parameters to the python script.

```bash
python3 get_validator_duties.py <validator index number(s)>
# Example
# python3 get_validator_duties.py 1000 1001 1002 1003
```

Sample Output showing the longest gap in seconds, \# of slots and time range.

> Longest gap \(first\):
>
> 120.0 seconds \(10 slots\), from 13:37:35 until 13:39:35

{% hint style="warning" %}
\*\*\*\*🔥 **Script Usage Caveats**: 

* This version does not include block proposals, although the odds of having one scheduled are really low of course. 
* Block proposal duties are only known for the current epoch, whereas attestation duties are known for the current and next one.
{% endhint %}

{% hint style="success" %}
Nice work. Now you now the best gap to avoid missing any validator duties.
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

## 🧩 Reference Material

{% embed url="https://gist.githubusercontent.com/pietjepuk2/eb021db978ad20bfd94dce485be63150/raw/cc874b3035f97495416353f203d70477b31ab05d/get\_validator\_duties.py" %}

[https://gist.github.com/mohamedmansour/9a82071802ffd58bef7ab5db530f23fd](https://gist.github.com/mohamedmansour/9a82071802ffd58bef7ab5db530f23fd
)

