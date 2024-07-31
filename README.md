import React, { useState, useEffect } from 'react';
import ViewForm from './ViewForm';

const ViewNewForm = ({ formName, processData, data, user, changeUser, programId, setProgramId, value }) => {
    const [count, setCount] = useState(0);
    const [progId, setProgId] = useState();
    const [dat, setDat] = useState();

    useEffect(() => {
        processData(dat);
    }, [dat, processData]);

    return (
        <div>
            <ViewForm
                processData={setDat}
                formName={`${formName}${value + 1}`}
                data={dat}
                count={count}
                setCount={setCount}
                programId={progId}
                setProgramId={setProgId}
            />
            <ViewForm2
                processData={setDat}
                formName={`${formName}${value + 1}`}
                data={dat}
                count={count}
                setCount={setCount}
                programId={progId}
                setProgramId={setProgId}
            />
            {/* Render individual properties */}
            <div>{`Form Name: ${formName}`}</div>
            <div>{`Project Name: ${projectName}`}</div>
            <div>{`Project Lead: ${projectLead}`}</div>
            <div>{`Objective: ${objective}`}</div>
            <div>{`PM: ${pm}`}</div>
        </div>
    );
}

export default ViewNewForm;
