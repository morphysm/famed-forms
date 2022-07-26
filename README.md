# famed-forms

This repository contains a prototype of the famed submission form.
The submission form is intended to be used as a Web3 replacement for Google Forms.

## How it Works

### Edit Form

Login with Metamask with an account with an address equal to NEXT_PUBLIC_OWNER.
Click on the `Edit` tab and make changes to the submission form. Currently, only text-based questions can be added. The submission form changes in this prototypical implementation are not saved beyond a page reload.

### Submit Form

Fill in the answers to the question in the `Respond` tab. Login with Metamask and click `Send Form`. You will be requested to approve the signing of the to-be-submitted message by Metamask. The form will be submitted using [Waku](https://wakuconnect.dev/). Messages might need several minutes to be transmitted. The backend will decrypt, verify the signature and log the transmitted message as soon as the Waku network forwards it.

## How to Use

**Tested with:**

- Node.js v18.4.0
- go 1.18

**Navigate to `/frontend`:**

```
cd frontend
```

**Install the frontend libraries:**

```
npm i
```

**Start the frontend:**

```
npm run dev
```

Go to `http://localhost:3000/generateEncyptionKey` and connect your Metamask Wallet. Open your Metamask Wallet and select the account you want to use to decrypt messages in the backend. Click generate on the web page. Copy the encyption key. Use the encyption key as the value for the `NEXT_PUBLIC_PUBLIC_ENCRYPTION_KEY` frontend env variable and the corresponding private key you can retrive from your Metamask Wallet as the value for the `ETH_PRIVATE_KEY` backend env variable.

**Note:** eth_getEncryptionPublicKey is [depreacted](https://medium.com/metamask/metamask-api-method-deprecation-2b0564a84686) an will be replaced as described in the [backlog](#Backlog).

**Set up a .local.env variable in `/frontend` containing:**

```
NEXT_PUBLIC_PUBLIC_ENCRYPTION_KEY=<Public encryption key for metamask based encryption, derived from ETH_PRIVATE_KEY>
NEXT_PUBLIC_OWNER=<Ethereum Address of the owner of the submission form, used to edit>
```

**Restart the fronend:**

```
ctl+c
npm run dev
```

**Open a new terminal and navigate to `/backend`:**

```
cd backend
```

**Start the backend:**

```
ETH_PRIVATE_KEY=<Ethereum private key for decryption retrived in previous step> go run main.go
```

## In Action

**Edit Form:**

<img width="1166" alt="Screenshot 2022-07-27 at 16 46 23" src="https://user-images.githubusercontent.com/11260050/181278085-baf52d57-e4fa-468d-9f23-62949f3210b9.png">

**Form:**

<img width="1164" alt="Screenshot 2022-07-27 at 16 45 51" src="https://user-images.githubusercontent.com/11260050/181278064-dc4d8273-e0a4-4a90-a4ea-302e5e187b3b.png">

**Sign Message:**

<img width="412" alt="Screenshot 2022-07-27 at 16 47 14" src="https://user-images.githubusercontent.com/11260050/181278305-f32d334b-af57-4ad4-892c-5b0a53169e3d.png">

**Backend Log:**

```
2022-07-27T14:27:25.694+0200    INFO    gowaku.node2.filter     received request        {"fullNode": false, "peer": "16Uiu2HAmVkKntsECaYfefR1V2yCR79CegLATuTPE6B9TxgxBiiiA"}
2022-07-27T14:27:25.694+0200    INFO    gowaku.node2.filter     received a message push {"fullNode": false, "peer": "16Uiu2HAmVkKntsECaYfefR1V2yCR79CegLATuTPE6B9TxgxBiiiA", "messages": 1}
{"questions":["What is you favourite meal?","What is your favourite programming language?"],"answers":["Pizza", "Go"]}

```

## Backlog

- Implement token requierents to submit/edit (ERC20, ERC721, Soulbound Tokens)
- Implement interfaces to forward submissions
- Persist submission form changes
- Extend submission form elements to feature parity with Google Forms
- Add Waku's native transport layer encryption
- Use forward secrecy enabled encryption schema
- Implement tribute to talk, pay or stake tokens to submit
- Implement acknowledgment of form submission
