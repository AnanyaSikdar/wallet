import React, { useState, useEffect } from 'react';
import { ethers } from 'ethers';

function WatchList({ walletAddress }) {
  const [tokens, setTokens] = useState([]);
  const [newToken, setNewToken] = useState('');
  const [balances, setBalances] = useState({});

  const addToken = () => {
    setTokens([...tokens, newToken]);
    setNewToken('');
  };

  useEffect(() => {
    const fetchBalances = async () => {
      const provider = new ethers.providers.Web3Provider(window.ethereum);
      const balances = {};
      for (const token of tokens) {
        const contract = new ethers.Contract(token.address, token.abi, provider);
        balances[token.symbol] = await contract.balanceOf(walletAddress);
      }
      setBalances(balances);
    };
    if (walletAddress) {
      fetchBalances();
    }
  }, [tokens, walletAddress]);

  return (
    <div>
      <h3>Watch List</h3>
      <input
        type="text"
        placeholder="Add token address"
        value={newToken}
        onChange={(e) => setNewToken(e.target.value)}
      />
      <button onClick={addToken}>Add Token</button>
      <ul>
        {tokens.map((token, index) => (
          <li key={index}>{token.symbol}: {balances[token.symbol]?.toString()}</li>
        ))}
      </ul>
    </div>
  );
}

export default WatchList;
