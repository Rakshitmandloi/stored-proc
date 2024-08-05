const handleSubmit = async () => {
  setLoading(true);
  setError(null);
  try {
    const response = await fetch('https://intranetws.nomuranow.com/snow-sso/access-token.json', {
      method: 'GET',
      headers: {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
        'Referer': 'http://localhost:3000/',  // Replace with your actual origin
        // You can add other headers if needed
      },
    });
    if (response.ok) {
      const data = await response.json();
      if (data.access_token) {
        console.log('Successful authentication');
        setUsername(inputUsername);
        setSelectedComponent(0);
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
