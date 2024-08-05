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
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
        'Cache-Control': 'max-age=0',
      },
      credentials: 'include', // Ensure cookies are included in the request
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
