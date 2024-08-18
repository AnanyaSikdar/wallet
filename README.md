import { useState } from 'react';
import { ethers } from 'ethers';

function WalletConnection() {
  const [walletAddress, setWalletAddress] = useState('');

  const connectWallet = async () => {
    if (window.ethereum) {
      const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
      setWalletAddress(accounts[0]);
    } else {
      alert('Please install Metamask!');
    }
  };

  const handleManualInput = (event) => {
    setWalletAddress(event.target.value);
  };

  return (
    <div>
      <button onClick={connectWallet}>Connect Metamask</button>
      <input
        type="text"
        placeholder="Enter wallet address"
        value={walletAddress}
        onChange={handleManualInput}
      />
      <p>Connected Wallet: {walletAddress}</p>
    </div>
  );
}

export default WalletConnection;
# wallet
