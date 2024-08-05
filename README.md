const handleSubmit = async () => {
  setLoading(true);
  setError(null);
  try {
    const response = await axios.get('https://intranetws.nomuranow.com/snow-sso/access-token.json', {
      headers: {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0',
      },
      withCredentials: true, // Include cookies
    });
    const data = response.data;
    if (data.access_token) {
      console.log('Successful authentication');
      setUsername(inputUsername); // Set the username after successful authentication
      setSelectedComponent(0); // Navigate to the main landing page by setting selectedComponent
    } else {
      setError('Sorry, you are not a valid user.');
    }
  } catch (err) {
    console.error('Error fetching data:', err);
    setError('Error checking user');
  } finally {
    setLoading(false);
  }
};
