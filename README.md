# Khan-
App bana 
// WithdrawalForm.jsx
import React, { useState } from 'react';
import axios from 'axios';

export default function WithdrawalForm({ token }) {
  const [amount, setAmount] = useState('');
  const [destination, setDestination] = useState('');
  const [loading, setLoading] = useState(false);
  const [msg, setMsg] = useState('');

  const submit = async (e) => {
    e.preventDefault();
    setLoading(true);
    setMsg('');
    try {
      const res = await axios.post('/api/withdraw/request',
        { amount, destination },
        { headers: { Authorization: `Bearer ${token}` } }
      );
      setMsg(`Request created. ID: ${res.data.withdrawalId}. Check inbox for OTP if required.`);
    } catch (err) {
      setMsg(err.response?.data?.error || 'Error');
    } finally { setLoading(false); }
  };

  return (
    <form onSubmit={submit}>
      <div>
        <label>Amount</label>
        <input type="number" step="0.01" value={amount} onChange={e => setAmount(e.target.value)} required />
      </div>
      <div>
        <label>Destination (UPI / Bank)</label>
        <input value={destination} onChange={e => setDestination(e.target.value)} required />
      </div>
      <button disabled={loading}>Withdraw</button>
      {msg &&
