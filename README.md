const handleSubmit = async () => {
  const username = inputUsername;  // Get username from state
  const password = inputPassword;  // Get password from state

  setLoading(true);
  setError(null);

  try {
    const xhr = new XMLHttpRequest();
    const url = 'https://intranetws.nomuranow.com/snow-sso/access-token.json';
    
    xhr.open('GET', url, true, username, password);
    xhr.withCredentials = true;
    
    xhr.onload = function() {
      if (xhr.status === 200) {
        const data = JSON.parse(xhr.responseText);
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
    };

    xhr.onerror = function() {
      setError('Error checking user');
    };

    xhr.send();
  } catch (err) {
    console.error('Error fetching data:', err);
    setError('Error checking user');
  } finally {
    setLoading(false);
  }
};
