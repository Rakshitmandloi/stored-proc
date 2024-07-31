const onboardingInfo = useCallback(() => {
    setLoading(true);
    fetchData().then((data) => {
        setData(data); // Ensure data is correctly set
        setLoading(false); // Only set loading to false after processing data
    });
}, []);
