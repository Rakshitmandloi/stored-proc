useEffect(() => {
  const fetchFlowDataForAllIds = async () => {
    console.log("triggered");
    setLoading(true);
    try {
      if (programFlows.length > 0) {
        const results = await Promise.all(
          programFlows.map(async (key) => {
            const response = await fetch(
              `api/query/GET_FLOW_DETAILS?flow_id=${key.id}&flow_version_id=${key.flow_version_id}`
            );
            const res = await response.json();
            console.log("res", res);
            return { [key.name]: res }; // Return an object with the key name as the property
          })
        );
        // Merge the results into a single object and update the state
        const newFlowData = results.reduce((acc, curr) => {
          return { ...acc, ...curr };
        }, {});
        setFlowData((prevState) => ({
          ...prevState,
          ...newFlowData,
        }));
        console.log(newFlowData);
      }
    } catch (err) {
      console.log('Error fetching flow data:', err);
    } finally {
      setLoading(false);
    }
  };

  fetchFlowDataForAllIds();
}, [programFlows]); // Ensure programFlows is in the dependency array
