import React, { useState } from 'react';
import ViewForm from './ViewForm';
import ViewForm2 from './ViewForm2';

const ViewNewForm = ({ formName, processData, data, user, changeUser, programId, setProgramId, value }) => {
  const [count, setCount] = useState(0);
  const [progId, setProgId] = useState(3);
  const [dat, setDat] = useState();

  const updateData = (newData) => {
    processData(newData); // Call the processData function passed down from the parent
    setDat(newData); // Update the local state
  };

  return (
    <>
      <ViewForm
        updateData={updateData}
        formName={`form${value + 1}`}
        data={dat}
        count={count}
        setCount={setCount}
        programId={progId}
        setProgramId={setProgId}
      />
      {dat && (
        <ViewForm2
          updateData={updateData}
          formName={`form${value + 1}`}
          data={dat}
          count={count}
          setCount={setCount}
          programId={progId}
          setProgramId={setProgId}
        />
      )}
    </>
  );
};

export default ViewNewForm;
