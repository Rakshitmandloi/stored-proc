import React from 'react';
import { Container, Paper, Typography, Box } from '@mui/material';

const ComingSoonPage = () => {
  return (
    <Container
      maxWidth="sm"
      sx={{
        height: '100vh',
        display: 'flex',
        justifyContent: 'center',
        alignItems: 'center',
      }}
    >
      <Paper
        elevation={3}
        sx={{
          padding: '20px',
          textAlign: 'center',
        }}
      >
        <Typography variant="h4" component="h1" gutterBottom>
          Coming Soon
        </Typography>
        <Typography variant="body1">
          We are working hard to bring you something amazing!
        </Typography>
      </Paper>
    </Container>
  );
};

export default ComingSoonPage;
