import React, { useState } from 'react';
import { Button, Dialog, DialogActions, DialogContent, DialogTitle } from '@mui/material';
import Form6 from './Form6';

const FlowList = ({ project, setValue }) => {
  const [isForm6Open, setIsForm6Open] = useState(false);

  const handleOpenForm6 = () => {
    setIsForm6Open(true);
  };

  const handleCloseForm6 = () => {
    setIsForm6Open(false);
  };

  return (
    <>
      {/* Other existing components */}
      
      <Button
        onClick={handleOpenForm6}
        variant="contained"
        color="secondary"
        sx={{ marginTop: 2 }}
      >
        Open Form 6
      </Button>

      <Dialog open={isForm6Open} onClose={handleCloseForm6}>
        <DialogTitle>Form 6</DialogTitle>
        <DialogContent>
          <Form6 />
        </DialogContent>
        <DialogActions>
          <Button onClick={handleCloseForm6} color="primary">
            Close
          </Button>
        </DialogActions>
      </Dialog>
    </>
  );
};

export default FlowList;
