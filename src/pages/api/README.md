# API Endpoints and Usage Guide

## Endpoints

### 1. `/api/swipe_right`

- **Purpose:** Initiates a swipe right action, typically used to indicate a positive interest in an item.
- **Method:** POST
- **Parameters:**
  - `clerkId`: The unique identifier of the Clerk user performing the action.
  - `objectId`: The unique identifier of the object being swiped right.
- **Usage Example:**
  ```javascript
  const handleSwipeRight = async () => {
    const objectId = {objectId};
    try {
      const response = await axios.post('/api/swipe_right', {
        clerkId: user.id,
        objectId: objectId
      });
      console.log('Response from /api/swipe_right:', response.data);
    } catch (error) {
      console.error('Error on swipe right:', error);
    }
  };
  ```

  ### 1b. `/api/swipe_left`

- **Purpose:** Initiates a swipe left action, typically used to indicate a lack of interest in an item.
- **Method:** POST
- **Parameters:**
  - `clerkId`: The unique identifier of the Clerk user performing the action.
  - `objectId`: The unique identifier of the object being swiped left.
- **Usage Example:**
  ```javascript
  const handleSwipeLeft = async () => {
    const objectId = '6683a5ed53238178ac59b76f';
    try {
      const response = await axios.post('/api/swipe_left', {
        clerkId: user.id,
        objectId: objectId
      });
      console.log('Response from /api/swipe_left:', response.data);
    } catch (error) {
      console.error('Error on swipe left:', error);
    }
  };
  ```

### 1c. `/api/liked`

- **Purpose:** Handles the action of liking an object, marking it as a preferred item.
- **Method:** POST
- **Parameters:**
  - `clerkId`: The unique identifier of the Clerk user performing the action.
  - `objectId`: The unique identifier of the object being liked.
- **Usage Example:**
```javascript
  const handleLiked = async () => {
    const objectId = '6683a5ed53238178ac59b76f';
    try {
      const response = await axios.post('/api/liked', {
        clerkId: user.id,
        objectId: objectId
      });
      console.log('Response from /api/liked:', response.data);
    } catch (error) {
      console.error('Error on liked:', error);
    }
  };
  ```

### 1d. `/api/delete_liked`

- **Purpose:** Handles the action of unliking an object, removing it from the list of preferred items.
- **Method:** DELETE
- **Parameters:**
  - `clerkId`: The unique identifier of the Clerk user performing the action.
  - `objectId`: The unique identifier of the object being unliked.
- **Usage Example:**
```javascript
  const handleDeleteLiked = async () => {
    const objectId = '6683a5ed53238178ac59b76f';
    try {
      const response = await axios.post('/api/delete_liked', {
        clerkId: user.id,
        objectId: objectId
      });
      console.log('Response from /api/delete_liked:', response.data);
    } catch (error) {
      console.error('Error on delete_liked:', error);
    }
  };
  ```

### 1c. `/api/check-liked`

- **Purpose:** Checks which recommendations were liked previously by the Clerk user.
- **Method:** POST
- **Parameters:**
  - \`clerkId\`: The unique identifier of the Clerk user performing the action.
  - \`objects\`: A list of objects to check.
- **Usage Example:**
```javascript
  const extractAndCheckRecommendations = async () => {
    const ids = objects.slice(0, 15).map(item => item._id);
    try {
      const response = await axios.post('/api/heck-liked', {
        clerkId: user.id,
        ids
      });
      console.log('Response from /api/check-liked:', response.data);
    } catch (error) {
      console.error('Error checking recommendations:', error);
    }
  };
  ```

### 2. `/api/load_data` DO NOT USE - IGNORE

- **Purpose:**  Loads data based on latitude and longitude, typically for location-based services.
- **Method:** GET
- **Parameters:**
  - \`latitude\`: Latitude of the location.
  - \`longitude\`: Longitude of the location.
- **Usage Example:**
  ```javascript
  const handleHitFor100 = async () => {
    const latitude = 34.0522;
    const longitude = -118.2437;
    try {
      const response = await axios.get(\`/api/load_data?latitude=\${latitude}&longitude=\${longitude}\`);
      console.log('Response from /api/load_data:', response.data);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };
  ```
### 3. `/api/generate-first-swipes`

- **Purpose:** Fetches random objects for initial display, used to provide the user with initial options.
- **Method:** GET
- **Usage Example:**
 ```javascript
  const fetchRandomObjects = async () => {
    try {
      const response = await axios.get('/api/generate-first-swipes');
      console.log('Random objects:', response.data);
      setRandomObjects(response.data);
    } catch (error) {
      console.error('Error fetching random objects:', error);
    }
  };
  ```

  ### 4. `/api/generate-recommendations/:clerkId`

- **Purpose:** Fetches 15 recommendations for a specific user when they are on the dashboard.
- **Method:** POST
- **Parameters:**
  - \`clerkId\`: The unique identifier of the Clerk user.
- **Usage Example:**
  ```javascript
  const fetchRecommendations = async () => {
    if (user) {
      try {
        const clerkId = user.id; 
        const url = \`/api/generate-recommendations/\${clerkId}\`;
        const response = await axios.post(url);
        console.log('Recommendations:', response.data);
        setRandomObjects(response.data);
      } catch (error) {
        console.error('Error fetching recommendations:', error);
      }
    } else {
      console.error('User not signed in');
    }
  };
  ```

### 5. `/api/change-to-embed` DO NOT USE - IGNORE

- **Purpose:** Updates embeddings, used internally for machine learning models.
- **Method:** POST
- **Usage Example:**
```javascript
  const updateEmbeddings = async () => {
    try {
      const response = await axios.post('/api/change-to-embed');
      console.log('Update Embeddings Response:', response.data);
    } catch (error) {
      console.error('Error updating embeddings:', error);
    }
  };
  ```

### 6. `/api/generate-single-recommendation/:clerkId`

- **Purpose:** Fetches a single recommendation of an item from different places for a specific user when they expand a card.
- **Method:** POST
- **Parameters:**
  - \`clerkId\`: The unique identifier of the Clerk user.

- **Request Body:**
  - \`latitude:\` Latitude of the user's current location.
  - \`longitude:\` Longitude of the user's current location.
  - \`timeOfDay:\` The time of day when the request is made (e.g., "Breakfast", "Lunch", "Dinner").

- **Response Body:**
  JSON object containing the recommended food item with various attributes.


- **Usage Example:**
```javascript
  const sendRecommendationRequest = async () => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(async (position) => {
        const latitude = position.coords.latitude;
        const longitude = position.coords.longitude;
        const timeOfDay = "Lunch";
        try {
          const response = await fetch(\'/api/generate-single-recommendation/\${user.id}\', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({ latitude, longitude, timeOfDay })
          });

          const result = await response.json();
          console.log(result);
        } catch (error) {
          console.error('Error sending recommendation request:', error);
        }
      }, (error) => {
        console.error('Error getting location', error);
      });
    } else {
      console.error('Geolocation is not supported by this browser.');
    }
  };
  ```

  ### `/api/thumbs_up`

- **Purpose:** Handles thumbs up action, indicating positive feedback for an item.
- **Method:** POST
- **Parameters:**
  - `clerkId`: The unique identifier of the Clerk user performing the action.
  - `objectId`: The unique identifier of the object being given a thumbs up.
- **Usage Example:**
  ```javascript
  const handleThumbsUp = async () => {
    const objectId = '6683a5ed53238178ac59b76f';
    try {
      const response = await axios.post('/api/thumbs_up', {
        clerkId: user.id,
        objectId: objectId
      });
      console.log('Response from /api/thumbs_up:', response.data);
    } catch (error) {
      console.error('Error on thumbs up:', error);
    }
  };
  ```

### `/api/thumbs_down`

- **Purpose:** Handles thumbs down action, indicating negative feedback for an item.
- **Method:** POST
- **Parameters:**
  - `clerkId`: The unique identifier of the Clerk user performing the action.
  - `objectId`: The unique identifier of the object being given a thumbs down.
- **Usage Example:**
  ```javascript
  const handleThumbsDown = async () => {
    const objectId = '6683a5ed53238178ac59b76f';
    try {
      const response = await axios.post('/api/thumbs_down', {
        clerkId: user.id,
        objectId: objectId
      });
      console.log('Response from /api/thumbs_down:', response.data);
    } catch (error) {
      console.error('Error on thumbs down:', error);
    }
  };
```
