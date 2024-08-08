import React from 'react';
import { Box, Typography, Stack } from '@mui/material';

const LegendItem = ({ color, label }) => (
  <Stack direction="row" alignItems="center" spacing={1}>
    <Box sx={{ width: 16, height: 16, backgroundColor: color, borderRadius: 1 }} />
    <Typography>{label}</Typography>
  </Stack>
);

const Legend = () => {
  return (
    <Stack direction="row" spacing={2}>
      <LegendItem color="green" label="Complete" />
      <LegendItem color="blue" label="In Progress" />
      <LegendItem color="grey" label="Yet to Begin" />
    </Stack>
  );
};

export default Legend;
Explanation:
LegendItem Component: This component represents a single legend item, containing a small colored box and a label. It uses the Box component to create the square with the specified background color and the Typography component to display the label.
Legend Component: This component uses the Stack component with direction="row" to arrange the LegendItem components in a horizontal line with spacing between them.
How to Use:
You can import and use the Legend component wherever you need it in your application.

javascript
Copy code
import Legend from './Legend';

function App() {
  return (
    <div>
      <Legend />
      {/* Other components */}
    </div>
  );
}

export default App;
Customization:
You can adjust the sx properties of the Box component to change the size of the colored squares.
You can add more LegendItem components to the Legend component to represent additional statuses or colors.
This setup will create a responsive, clean-looking legend with colored squares aligned next to their corresponding labels.









