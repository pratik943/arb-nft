# ArbiNFT Mini App

A simple Farcaster Mini App that lets users mint a **free Arbitrum NFT** directly inside the Farcaster mobile/web app, view total supply on-chain, share their mint to Warpcast (Farcaster), and switch networks.

---

## 🚀 Features

- **One-click mint** of a free NFT  
- **Live total supply** display (reads `totalSupply()` from your contract)  
- **“Share on Farcaster”** button that opens the Farcaster web composer  
- **Network selector** so users can switch between Base, Arbitrum, or another EVM chain  
- **Responsive mobile-first UI** matching Farcaster’s mini-app viewport  
- **Live ARB/USD price chart** via TradingView  

---

## 📁 Repository Structure

```
/
├─ index.html            ← Mini-app entrypoint  
├─ contract.json         ← Contract address & minimal ABI  
├─ README.md             ← This file  
└─ assets/               ← image.png, splash.png, icon.png, og.png  
```

---

## 🔧 Setup & Usage

1. **Fork this repo** to your own GitHub account.  
2. Clone locally:
   ```bash
   git clone https://github.com/YOURNAME/arb-nft.git
   cd arb-nft
   ```
3. **Edit `contract.json`** with your contract’s address & ABI:
   ```json
   {
     "address": "0xYOUR_CONTRACT_ADDRESS",
     "abi": [
       { "inputs": [], "name": "mint", "outputs": [], "stateMutability": "nonpayable", "type": "function" },
       { "inputs": [], "name": "totalSupply", "outputs": [{ "type": "uint256" }], "stateMutability": "view", "type": "function" },
       { "inputs": [{ "type": "uint256" }], "name": "tokenURI", "outputs": [{ "type": "string" }], "stateMutability": "view", "type": "function" }
     ]
   }
   ```
4. **Configure your assets** under `/assets`:
   - `image.png` (3:2 ratio, 600×400–3000×2000px)  
   - `splash.png` (Farcaster splash requirements)  
   - `icon.png` (square logo)  
   - `og.png` (social preview)  
5. **Add a network selector** to `index.html`:
   ```html
   <select id="networkSelect">
     <option value="arbitrum">Arbitrum</option>
     <option value="base">Base</option>
     <option value="mainnet">Ethereum</option>
   </select>
   ```
   And update your JavaScript:
   ```js
   import { createConfig, connect } from '@wagmi/core';
   import { http } from '@wagmi/core/transports';
   import { arbitrum, base, mainnet } from '@wagmi/core/chains';
   import { farcasterMiniApp } from '@farcaster/miniapp-wagmi-connector';

   const chainMap = { arbitrum, base, mainnet };
   const networkSelect = document.getElementById('networkSelect');

   let config = createConfig({
     chains: [arbitrum],
     transports: { [arbitrum.id]: http() }
   });

   networkSelect.addEventListener('change', async (e) => {
     const chain = chainMap[e.target.value];
     config = createConfig({
       chains: [chain],
       transports: { [chain.id]: http() }
     });
     await connect(config, { connector: farcasterMiniApp() });
     // re-fetch totalSupply, etc.
   });
   ```
6. **Deploy** to Vercel, Netlify, or GitHub Pages over HTTPS.  
7. **Register** on https://miniapps.farcaster.xyz using your `index.html` URL.  
8. **Test** in the Farcaster mobile app or Warpcast web.

---

## 🤖 Need help? ChatGPT prompt template

```
I have a Farcaster Mini App at https://YOURDOMAIN.com/index.html. I want to:
1. Add a network selector (Base, Arbitrum, Mainnet) with dynamic Wagmi config.
2. Display ownerOf(tokenId) after mint.
3. Customize the mint button animation.
4. Integrate a live price feed for multiple tokens.

Here's my current `index.html`:
[paste your `index.html` here]

Can you provide a step-by-step guide and updated code?
```

---

## 🤝 Contributing

1. Fork  
2. Create branch (`git checkout -b feature/foo`)  
3. Commit (`git commit -am 'Add foo'`)  
4. Push (`git push origin feature/foo`)  
5. Open a Pull Request  

---

## 📜 License

MIT  

---

Happy minting, casting & network switching! 🚀  
