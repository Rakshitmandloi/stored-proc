# sql.py

from sqlalchemy import create_engine, text
from sqlalchemy.orm import sessionmaker
import pandas as pd

class DBConnections:
    def __init__(self, server, port, password, user, db):
        self.conn = create_engine(f'mysql+pymysql://{user}:{password}@{server}:{port}/{db}')
        self.cached_sql = pd.DataFrame()

    def fetch(self, query):
        print(f"Query: {query}")
        return pd.read_sql(query, self.conn)

    def fetch_query(self, process):
        if len(self.cached_sql) == 0 or len(self.cached_sql[self.cached_sql["process_name"] != process]) == 0:
            self.cached_sql = self.fetch('select * from f2b_query')
        return self.cached_sql.loc[self.cached_sql["process_name"] == process, 'query'].item()

    def fetch_data(self, process, parameters={}):
        query = self.fetch_query(process).format(**parameters)
        return self.fetch(query).to_dict(orient='records')

    def execute_stored_proc(self, procedure_name, params):
        sql_query = text(f"CALL {procedure_name}(:a, :b, :c, :d, :e)")
        variables = {
            "a": params[0],
            "b": params[1],
            "c": params[2],
            "d": params[3],
            "e": "mandloir"
        }
        with self.conn.connect() as connection:
            connection.execute(sql_query, variables)
            connection.commit()

    def check_user_exists(self, username):
        query = f"SELECT COUNT(*) FROM users WHERE user_name = '{username}'"
        result = self.fetch(query)
        return result.iloc[0, 0] > 0

    def add_user(self, user_name, role, fname, lname, email):
        sql_query = text("CALL AddUser(:user_name, :role, :fname, :lname, :email)")
        variables = {
            'user_name': user_name,
            'role': role,
            'fname': fname,
            'lname': lname,
            'email': email
        }
        with self.conn.connect() as connection:
            connection.execute(sql_query, variables)
            connection.commit()

    def get_user_details_from_api(self, user_id, password):
        import requests
        url = "https://api.example.com/get_user_details"
        response = requests.post(url, json={"user_id": user_id, "password": password})
        if response.status_code == 200:
            return response.json()
        else:
            return None

# Example usage
db = DBConnections(
    server=os.getenv("DB_SERVER"),
    port=os.getenv("DB_PORT"),
    password=os.getenv("DB_PASSWORD"),
    user=os.getenv("DB_USER"),
    db=os.getenv("DB_DB")
)


# Example usage
db = DBConnections(
    server=getenv("DB_SERVER"),
    port=getenv("DB_PORT"),
    password=getenv("DB_PASSWORD"),
    user=getenv("DB_USER"),
    db=getenv("DB_DB")
)


# server.py

from flask import Flask, request, jsonify
from sql import DBConnections
import os

app = Flask(__name__)
db = DBConnections(
    server=os.getenv("DB_SERVER"),
    port=os.getenv("DB_PORT"),
    password=os.getenv("DB_PASSWORD"),
    user=os.getenv("DB_USER"),
    db=os.getenv("DB_DB")
)

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

