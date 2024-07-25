# sql.py

from sqlalchemy import create_engine, text
from sqlalchemy.orm import sessionmaker
import requests

class Database:
    def __init__(self, connection_string):
        self.engine = create_engine(connection_string)
        self.Session = sessionmaker(bind=self.engine)

    def check_user_exists(self, username):
        query = text("SELECT COUNT(*) FROM users WHERE user_name = :username")
        with self.Session() as session:
            result = session.execute(query, {'username': username}).scalar()
            return result > 0

    def add_user(self, user_name, role, fname, lname, email):
        query = text("CALL AddUser(:user_name, :role, :fname, :lname, :email)")
        with self.Session() as session:
            session.execute(query, {
                'user_name': user_name,
                'role': role,
                'fname': fname,
                'lname': lname,
                'email': email
            })
            session.commit()

    def get_user_details_from_api(self, user_id, password):
        url = "https://api.example.com/get_user_details"
        response = requests.post(url, json={"user_id": user_id, "password": password})
        if response.status_code == 200:
            return response.json()
        else:
            return None

# server.py

from flask import Flask, request, jsonify
from sql import Database

app = Flask(__name__)
db = Database('sqlite:///your_database.db')

@app.route('/check_user', methods=['POST'])
def check_user():
    username = request.json.get('username')
    if db.check_user_exists(username):
        return jsonify({"exists": True})
    else:
        return jsonify({"exists": False})

@app.route('/add_user', methods=['POST'])
def add_user():
    user_details = request.json
    db.add_user(user_details['user_name'], user_details.get('role'), user_details.get('fname'), user_details.get('lname'), user_details.get('email'))
    return jsonify({"success": True})

@app.route('/get_user_details', methods=['POST'])
def get_user_details():
    user_id = request.json.get('user_id')
    password = request.json.get('password')
    user_details = db.get_user_details_from_api(user_id, password)
    if user_details:
        return jsonify(user_details)
    else:
        return jsonify({"error": "User details not found"}), 404

if __name__ == '__main__':
    app.run(debug=True)

import React, { useState, useEffect } from 'react';
import { Container, Box, Typography, Card, CardActionArea, Grid, CardMedia, AppBar, Toolbar, CircularProgress, Button } from '@mui/material';
import { styled, createTheme, ThemeProvider } from '@mui/material/styles';
import axios from 'axios';

import FlowCatalogView from './components/FlowCatalogView';
import ViewProgramView from './components/ViewProgramView';
import SystemAvailabilityView from './components/SystemAvailabilityView';
import ProgramCalenderView from './components/ProgramCalenderView';
import AddEditProgramView from './components/AddEditProgramView';
import './App.css';

const theme = createTheme({
  palette: {
    primary: {
      main: '#b0bec5',
    },
    secondary: {
      main: '#BABABA',
    },
  },
});

const features = [
  { name: 'View Program', component: 1, icon: 'https://via.placeholder.com/150' },
  { name: 'Add/Edit Program', component: 2, icon: 'https://via.placeholder.com/150' },
  { name: 'Flow Catalog', component: 3, icon: 'https://via.placeholder.com/150' },
  { name: 'Program Calendar', component: 4, icon: 'https://via.placeholder.com/150' },
  { name: 'System Availability', component: 5, icon: 'https://via.placeholder.com/150' },
];

const defaultImage = 'https://via.placeholder.com/150';

const StyledCard = styled(Card)({
  backgroundColor: 'rgba(0, 0, 0, 0.5)',
  color: 'white',
  height: '100%',
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  borderRadius: 16,
  transition: 'background-color 0.3s ease',
  '&:hover': {
    backgroundColor: 'grey',
  },
});

const CenteredCardActionArea = styled(CardActionArea)({
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'center',
  height: '100%',
  flexDirection: 'column',
});

const LandingPageContent = ({ onCardClick }) => (
  <Container sx={{ display: 'flex', flexDirection: 'column', height: '100vh' }}>
    <Box sx={{ flexGrow: 1, display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
      <Grid container spacing={2} justifyContent="center" alignItems="center">
        {features.map((feature) => (
          <Grid item xs={12} md={4} lg={3} key={feature.name} sx={{ height: '200px' }}>
            <StyledCard>
              <CenteredCardActionArea onClick={() => onCardClick(feature.component)}>
                <CardMedia
                  component="img"
                  image={feature.icon}
                  title={feature.name}
                  sx={{ height: '70%', width: '100%', objectFit: 'cover', mb: 1 }}
                />
                <Typography variant="h6">{feature.name}</Typography>
              </CenteredCardActionArea>
            </StyledCard>
          </Grid>
        ))}
      </Grid>
    </Box>
  </Container>
);

const LandingPage = () => {
  const [selectedComponent, setSelectedComponent] = useState(0);
  const [username, setUsername] = useState('mandloir');
  const [loading, setLoading] = useState(true);
  const [userExists, setUserExists] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const checkUser = async () => {
      try {
        const response = await axios.post('/check_user', { username });
        if (response.data.exists) {
          setUserExists(true);
        } else {
          const userDetailsResponse = await axios.post('/get_user_details', { user_id: 'your_user_id', password: 'your_password' });
          const userDetails = userDetailsResponse.data;
          if (userDetails) {
            await axios.post('/add_user', userDetails);
            setUserExists(true);
          } else {
            setError('User details not found');
          }
        }
      } catch (err) {
        setError('Error checking user');
      } finally {
        setLoading(false);
      }
    };

    checkUser();
  }, [username]);

  const handleButtonClick = (component) => {
    setSelectedComponent(component);
  };

  const renderComponent = () => {
    switch (selectedComponent) {
      case 1:
        return <ViewProgramView />;
      case 2:
        return <AddEditProgramView />;
      case 3:
        return <FlowCatalogView />;
      case 4:
        return <ProgramCalenderView />;
      case 5:
        return <SystemAvailabilityView />;
      default:
        return <LandingPageContent onCardClick={handleButtonClick} />;
    }
  };

  return (
    <ThemeProvider theme={theme}>
      <AppBar position="static" color="default">
        <Toolbar>
          {loading ? (
            <CircularProgress color="inherit" />
          ) : userExists ? (
            <>
              <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
                Welcome, {username}!
              </Typography>
              {selectedComponent !== 0 && (
                <Button color="inherit" onClick={() => setSelectedComponent(0)}>
                  Back to Home
                </Button>
              )}
            </>
          ) : (
            <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
              {error ? error : 'Adding user to database...'}
            </Typography>
          )}
        </Toolbar>
      </AppBar>
      {userExists && renderComponent()}
    </ThemeProvider>
  );
};

export default LandingPage;

