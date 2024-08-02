const MyTab = styled(Tab)(({ theme }) => ({
  '&.Mui-selected': {
    color: '#052D80',
    fontWeight: 'bold',
    backgroundSize: 'cover',  // Ensure the image covers the entire tab
    backgroundRepeat: 'no-repeat',  // Prevent image repetition
    backgroundPosition: 'center',  // Center the image within the tab
  },
}));

const Header = ({ headers, onChange, value }) => {
  return (
    <AppBar position="sticky" sx={{ width: '100vw', top: 0, background: 'white', left: 0 }}>
      <MyTabs value={value} onChange={onChange} centered>
        {headers.map((header) => (
          <MyTab 
            label={header} 
            key={header} 
            sx={{ 
              backgroundImage: `url(${Image})`, 
              backgroundSize: 'cover',  // Cover the entire tab area
              backgroundRepeat: 'no-repeat',  // Prevent repetition of the image
              backgroundPosition: 'center',  // Align the background image in the center of the tab
            }} 
          />
        ))}
      </MyTabs>
    </AppBar>
  );
};

export default Header;
