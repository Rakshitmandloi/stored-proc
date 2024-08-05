import { encode } from 'base-64';

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

  const authHeader = 'Basic ' + encode(username + ':' + password);

  setLoading(true);
  setError(null);

  try {
    const response = await fetch('https://intranetws.nomuranow.com/snow-sso/access-token.json', {
      method: 'GET',
      headers: {
        'Authorization': authHeader,
        'User-Agent': 'PostmanRuntime/7.29.0',
        'Accept': '*/*',
        'Accept-Encoding': 'gzip, deflate, br',
        'Connection': 'keep-alive',
        'Cookie': 'SSOAuthCoreprd=oTD-4EqEtQoQByWwdWk8r3GCwR4.*AAJTSQACMDQAAlNLABxHNzlhN0NUSzRoc0tPVHU3K3ZVMWRReG1yaGc9AAR0eXBIAANDVFMAAIMxAxAlxNA..;', // Include your session-related cookies here
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
