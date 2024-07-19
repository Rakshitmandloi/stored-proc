import React from 'react';
import { Container, Box, Typography, ImageList, ImageListItem, ImageListItemBar, Grid, Paper } from '@mui/material';
import { styled } from '@mui/system';

const features = [
  { name: 'View Program' },
  { name: 'Add/Edit Program' },
  { name: 'Flow Catalog' },
  { name: 'Program Calendar' },
  { name: 'System Availability' },
];

const StyledImageListItem = styled(ImageListItem)({
  backgroundColor: 'rgba(0, 0, 0, 0.5)',
  color: 'white',
  height: 150,
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  borderRadius: 16,
  transition: 'background-color 0.3s ease',
  '&:hover': {
    backgroundColor: 'red',
  },
});

function LandingPage() {
  const handleButtonClick = (feature) => {
    alert(`${feature} clicked`);
  };

  return (
    <Container>
      <Box sx={{ textAlign: 'center', my: 4 }}>
        <Typography variant="h4" component="div" gutterBottom>
          Welcome, User!
        </Typography>
        <Box sx={{ display: 'flex', justifyContent: 'center' }}>
          <ImageList sx={{ width: 800 }} cols={3} gap={24}>
            {features.map((feature) => (
              <StyledImageListItem key={feature.name} component={Paper} onClick={() => handleButtonClick(feature.name)}>
                <ImageListItemBar
                  title={<Typography variant="h6">{feature.name}</Typography>}
                  position="center"
                  sx={{
                    background: 'none',
                    textAlign: 'center',
                    '& .MuiImageListItemBar-title': {
                      fontSize: '1.25rem',
                    },
                  }}
                />
              </StyledImageListItem>
            ))}
          </ImageList>
        </Box>
      </Box>
    </Container>
  );
}

export default LandingPage;
