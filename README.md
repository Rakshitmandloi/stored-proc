const handleSubmit = async () => {
  setLoading(true);
  setError(null);
  try {
    const response = await fetch('https://intranetws.nomuranow.com/snow-sso/access-token.json', {
      method: 'GET',
      headers: {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0',
        'Accept': 'application/json',
      },
      credentials: 'include', // This ensures cookies are sent with the request
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
