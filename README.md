 useEffect(() => {
    if (processData && typeof processData === 'function') {
      processData(dat);
    } else {
      console.error("processData is not a function");
    }
  }, [dat, processData]);
