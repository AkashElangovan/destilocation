# destilocation
app like appls location feature
<!DOCTYPE html>
<html>
<head>
    <title>Location Based SMS</title>
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_GOOGLE_MAPS_API_KEY&libraries=places"></script>
    <script>
        let targetLocation = { lat: 37.7749, lng: -122.4194 }; // Set your target location (latitude and longitude)
        let threshold = 0.01; // Distance threshold in degrees

        function success(position) {
            const userLocation = {
                lat: position.coords.latitude,
                lng: position.coords.longitude
            };

            const distance = Math.sqrt(Math.pow(userLocation.lat - targetLocation.lat, 2) + Math.pow(userLocation.lng - targetLocation.lng, 2));

            if (distance < threshold) {
                sendSMS();
            }
        }

        function error() {
            console.log('Unable to retrieve your location');
        }

        function checkLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(success, error);
            } else {
                console.log('Geolocation is not supported by your browser');
            }
        }

        function sendSMS() {
            fetch('/send-sms', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    message: 'User has reached the target location',
                    to: 'TARGET_PHONE_NUMBER' // Replace with the recipient's phone number
                })
            })
            .then(response => response.json())
            .then(data => {
                console.log('SMS sent:', data);
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }

        setInterval(checkLocation, 30000); // Check location every 30 seconds
    </script>
</head>
<body>
    <h1>Location Based SMS</h1>
    <p>Keep this page open to check location and send SMS when the target is reached.</p>
</body>
</html>
