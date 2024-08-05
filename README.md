import React, { useState } from 'react';
import { Container, Typography, TextField, Button, LinearProgress } from '@mui/material';

const WelcomePage = ({ username, setUsername, setSelectedComponent }) => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const handleInputChange = (e) => {
    setUsername(e.target.value);
  };

  const handleSubmit = async () => {
    setLoading(true);
    setError(null); // Reset error state
    try {
      const response = await fetch(`https://intranetws.nomuranow.com/snow-sso/access-token.json`);
      if (response.ok) {
        const data = await response.json();
        if (data.access_token) {
          console.log('Successful authentication');
          setSelectedComponent(0); // Set selected component to 0 (or any other value as needed)
        } else {
          setError('Sorry, you are not a valid user.');
        }
      } else {
        setError('User details not found');
      }
    } catch (err) {
      setError('Error checking user');
    } finally {
      setLoading(false);
    }
  };

  return (
    <Container>
      <Typography variant="h4" gutterBottom>
        Welcome! Please enter your Username
      </Typography>
      <TextField
        label="Username"
        variant="outlined"
        fullWidth
        value={username}
        onChange={handleInputChange}
        disabled={loading}
      />
      {loading && <LinearProgress style={{ marginTop: '20px' }} />}
      <Button
        variant="contained"
        color="primary"
        onClick={handleSubmit}
        disabled={loading}
        style={{ marginTop: '20px' }}
      >
        Submit
      </Button>
      {error && (
        <Typography color="error" style={{ marginTop: '20px' }}>
          {error}
        </Typography>
      )}
    </Container>
  );
};

export default WelcomePage;
