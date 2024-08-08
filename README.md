const handleSubmit = async () => {
  const username = inputUsername;  // Get username from state
  const password = inputPassword;  // Get password from state

  if (username.trim() === '' || password.trim() === '') {
    if (username.trim() === '') {
      setUsernameError(true);
    }
    if (password.trim() === '') {
      setPasswordError(true);
    }
    return;
  }

  setLoading(true);
  setError(null);

  try {
    // Authentication API Call
    const authResponse = await axios.post('/authentication', { username, password });

    if (authResponse.status === 200) {
      const data = await authResponse.data;
      if (data.access_token) {
        console.log('Successful authentication');
        setUsername(inputUsername);
        setSelectedComponent(0);

        // Check if user exists in the database
        const checkUserResponse = await axios.post('/check_user', { username });

        if (checkUserResponse.status === 200 && checkUserResponse.data.exists) {
          console.log('User exists in the database');
        } else {
          console.log('User does not exist, adding to database');

          // Add user to the database
          const addUserResponse = await axios.post('/add_user', {
            user_name: username,
            // Assuming you have these details from somewhere in your application
            role: 'user',
            fname: 'John',  // Replace with actual data
            lname: 'Doe',   // Replace with actual data
            email: 'john.doe@example.com' // Replace with actual data
          });

          if (addUserResponse.status === 200) {
            console.log('User added to the database');
          } else {
            console.error('Failed to add user');
            setError('Failed to add user to the database');
          }
        }
      } else {
        setError('Sorry, you are not a valid user.');
      }
    } else {
      setError('User details not found');
    }
  } catch (err) {
    console.error('User details not found', err);
    setError('User details not found');
  } finally {
    setLoading(false);
  }
};
