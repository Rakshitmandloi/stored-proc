import React, { useState } from 'react';
import { Container, Typography, TextField, Button, LinearProgress, Paper, Box, Grid } from '@mui/material';

const WelcomePage = ({ setUsername, setSelectedComponent }) => {
  const [inputUsername, setInputUsername] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const handleInputChange = (e) => {
    setInputUsername(e.target.value);
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
          setUsername(inputUsername); // Set the username after successful authentication
          setSelectedComponent(0); // Navigate to the main landing page by setting selectedComponent
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
            />
            {loading && <LinearProgress style={{ marginTop: '20px' }} color="secondary" />}
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
