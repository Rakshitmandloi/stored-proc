import React, { useState } from 'react';
import { Container, Typography, TextField, Button, LinearProgress, Paper, Box, Grid } from '@mui/material';

const WelcomePage = ({ setUsername, setSelectedComponent }) => {
  const [inputUsername, setInputUsername] = useState('');
  const [inputPassword, setInputPassword] = useState(''); // New state for password
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [usernameError, setUsernameError] = useState(false);
  const [passwordError, setPasswordError] = useState(false);

  const handleInputChange = (e) => {
    setInputUsername(e.target.value);
    if (usernameError) {
      setUsernameError(false); // Reset error when user starts typing
    }
  };

  const handlePasswordChange = (e) => {
    setInputPassword(e.target.value);
    if (passwordError) {
      setPasswordError(false); // Reset error when user starts typing
    }
  };

  const handleSubmit = async () => {
    if (inputUsername.trim() === '' || inputPassword.trim() === '') {
      if (inputUsername.trim() === '') {
        setUsernameError(true);
      }
      if (inputPassword.trim() === '') {
        setPasswordError(true);
      }
      return;
    }

    const authHeader = 'Basic ' + btoa(`${inputUsername}:${inputPassword}`);

    setLoading(true);
    setError(null); // Reset error state
    try {
      const response = await fetch('https://intranetws.nomuranow.com/snow-sso/access-token.json', {
        method: 'GET',
        headers: {
          'Authorization': authHeader,
          'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0',
          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
        },
        credentials: 'include', // Ensures cookies are included in the request
      });

      if (response.ok) {
        const data = await response.json();
        if (data.access_token) {
          console.log('Successful authentication');
          setUsername(inputUsername); // Set the username after successful authentication
          setSelectedComponent(0); // Navigate to the main landing page by setting selectedComponent
        } else {
          setError('Sorry, you are not a valid user.');
        }
      } else {
        setError('User details not found');
      }
    } catch (err) {
      console.error('Error fetching data:', err);
      setError('Error checking user');
    } finally {
      setLoading(false);
    }
  };

  return (
    <Container>
      <Grid
        container
        style={{ minHeight: '100vh' }}
        alignItems="center"
        justifyContent="center"
      >
        <Grid item xs={10} sm={8} md={4}>
          <Paper elevation={10} style={{ padding: '30px', borderRadius: '10px' }}>
            <Box mb={3}>
              <Typography variant="h5" align="center" gutterBottom>
                Login
              </Typography>
            </Box>
            <TextField
              label="Username"
              variant="outlined"
              fullWidth
              value={inputUsername}
              onChange={handleInputChange}
              disabled={loading}
              error={usernameError}
              helperText={usernameError ? 'Username is required' : ''}
            />
            <TextField
              label="Password"
              variant="outlined"
              fullWidth
              type="password" // Set input type to password
              value={inputPassword}
              onChange={handlePasswordChange}
              disabled={loading}
              error={passwordError}
              helperText={passwordError ? 'Password is required' : ''}
              style={{ marginTop: '20px' }}
            />
            {loading && <LinearProgress style={{ marginTop: '20px' }} />}
            <Button
              variant="contained"
              fullWidth
              onClick={handleSubmit}
              disabled={loading}
              style={{ marginTop: '20px', backgroundColor: '#c32828', color: 'white' }}
            >
              LOGIN
            </Button>
            {error && (
              <Typography color="error" style={{ marginTop: '20px', textAlign: 'center' }}>
                {error}
              </Typography>
            )}
          </Paper>
        </Grid>
      </Grid>
    </Container>
  );
};

export default WelcomePage;
