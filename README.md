# **Spring of the Hearts (Quran Shorts) App**

**An effortless and immersive way to connect with the Quran daily through dynamic audio-visual recitations.**

## **Table of Contents**

1. [About the Project](#about-the-project)
2. [Features](#features)
3. [Technical Stack](#technical-stack)
4. [Architecture Overview](#architecture-overview)
5. [Setup & Installation](#setup--installation)
6. [Usage](#usage)
7. [Future Enhancements](#future-enhancements)
8. [License](#license)

## **1. About the Project**

The "Spring of the Hearts" (ربيع القلوب / Rabee Alqolob) app, also referred to as "Quran Shorts," transforms how users engage with the Holy Quran. Moving beyond traditional list-based browsing, this application provides a **highly accessible, "one-click" stream** of curated Quranic recitations. Inspired by platforms like TikTok and YouTube Shorts, the app aims to deliver a continuously fresh and deeply immersive audio-visual experience.

**Our Vision:** To lower the barrier to daily Quranic engagement, foster consistent connection, and enhance spiritual immersion by presenting the Quran in a dynamic, easy-to-consume format, embodying the spiritual renewal suggested by "Spring of the Hearts."

## **2. Features**

### **Core Experience:**

-   **One-Click Playback:** The app starts playing a Quranic "short" immediately upon launch, eliminating decision fatigue.
-   **Dynamic Audio-Visual Feed:** Each "short" pairs Quranic audio recitation with complementary background visuals for an immersive atmosphere.
-   **"Random, Not Seen" Priority:** The app serves content that the user hasn't previously experienced.
-   **"Oldest Seen" Fallback:** Once all available "shorts" have been viewed, it transitions to the oldest-seen content first, ensuring a continuous stream.
-   **Seamless Swiping Navigation:** Users can effortlessly swipe vertically to move between "shorts," mimicking popular short-form video interactions.
-   **Background Audio:** Recitations continue to play even when the app is minimized or the device screen is off.

### **Technical Capabilities:**

-   **Optimized for Speed:** Leveraging Cloudflare CDN for media delivery ensures minimal buffering and "instant start" playback.
-   **User-Specific History:** A backend system tracks each user's viewing history to power the "random, not seen" logic.

## **3. Technical Stack**

The application is built with a modern, scalable stack to deliver a smooth and responsive experience:

-   **Frontend:**
    -   **Expo React Native:** For cross-platform mobile development (iOS and Android) from a single codebase, leveraging JavaScript/TypeScript.
    -   **expo-av:** For robust audio and video playback, including background audio and media controls.
-   **Backend:**
    -   **(Your Preferred Backend Framework/Language):** e.g., Node.js with Express, Python with Flask/Django, or a serverless function platform.
    -   **MongoDB:** NoSQL database used to store reading content (audio/video URLs) and track user-specific seen history.
-   **Content Delivery Network (CDN):**
    -   **Cloudflare:** To cache and deliver video and audio assets globally, ensuring low latency and high availability for a fast start and play experience.
-   **External APIs:**
    -   **Quranic Content APIs:** To source Quranic audio files (e.g., Al-Quran Cloud API) and potentially video content URLs.

## **4. Architecture Overview**

The app's architecture is designed for dynamic content delivery and a seamless user experience:

1. **Content Ingestion:** Reading objects are populated in a MongoDB collection. Each Reading can contain a single `videoUrl` (for combined audio-visual content) or separate `audioUrl` and `videoUrl` fields (for distinct audio and visual tracks played simultaneously).
2. **User Authentication (Implicit/Simple):** A `userId` (could be a simple anonymous ID for starters) identifies the user.
3. **Frontend Request:** When the user opens the app or swipes for a new "short," the Expo React Native app sends a request to the backend, including the `userId`.
4. **Backend Logic:**
    - The backend queries MongoDB for Reading entries that the specified `userId` has _not_ seen.
    - If unseen Readings are found, one is selected randomly.
    - If all Readings have been seen, the backend identifies the Reading that was seen the _longest time ago_ by that user.
    - The selected Reading is marked as "seen" with a current timestamp for that `userId` in MongoDB.
    - The `audioUrl` and `videoUrl` for the selected Reading are sent back to the app. The app determines how to play them based on the content structure.
5. **Client-Side Playback (Expo):**
    - The Expo app receives the `audioUrl` and `videoUrl`.
    - It immediately starts playing the media. If a single `videoUrl` containing both audio and video is provided, it plays that. If separate `audioUrl` and `videoUrl` are provided, it plays the audio recitation and the video (likely muted or with low volume) concurrently using `expo-av`.
    - While the current "short" plays, the app pre-fetches the next "short" from the backend, ensuring instant transitions upon swipe.
6. **CDN Acceleration (Cloudflare):** All `audioUrl` and `videoUrl` requests are served via Cloudflare's CDN, ensuring optimal performance and global delivery.

## **5. Setup & Installation**

To run this project locally, you will need to set up both the frontend (React Native) and a basic backend (for content delivery and user history).

### **Prerequisites:**

-   Node.js (LTS version recommended)
-   npm or Yarn
-   Expo CLI (`npm install -g expo-cli`)
-   MongoDB instance (Local or MongoDB Atlas)
-   Access to a Quran API (e.g., Al-Quran Cloud)
-   Cloudflare account (optional for initial development, but highly recommended for production performance)

### **5.1. Backend Setup:**

_(This is a conceptual outline; actual implementation details depend on your chosen backend framework)_

1. **Clone the Backend Repository:**

    ```bash
    git clone [your-backend-repo-url]
    cd [your-backend-repo-name]
    ```

2. **Install Dependencies:**

    ```bash
    npm install # or yarn install
    ```

3. **Configure Environment Variables:**  
   Create a `.env` file in the backend root with:

    ```
    MONGODB_URI=your_mongodb_connection_string
    # Optionally: API_KEY_FOR_QURAN_API=your_api_key (if the chosen Quran API requires one)
    ```

4. **Initialize MongoDB Data (Optional but Recommended):**  
   You'll need a script or a process to populate your MongoDB with initial Reading documents (each containing `audioUrl` and `videoUrl`).

5. **Start the Backend Server:**
    ```bash
    npm start # or specific command for your backend
    ```

### **5.2. Frontend Setup:**

1. **Clone the Frontend Repository:**

    ```bash
    git clone [this-repo-url]
    cd [this-repo-name]
    ```

2. **Install Dependencies:**

    ```bash
    npm install # or yarn install
    ```

3. **Configure API Endpoint:**  
   You might need to update a constant in your React Native code (e.g., `src/config.js` or directly in `App.js` for simplicity) to point to your local or deployed backend API URL.

    ```javascript
    // Example: (inside your React Native project)
    const BACKEND_API_URL = "http://localhost:3000/api" // Or your deployed URL
    ```

4. **Start the Expo Development Server:**

    ```bash
    expo start
    ```

    This will open a new tab in your browser with Expo Dev Tools. You can then scan the QR code with your phone (Expo Go app required) or run it on an emulator/simulator.

## **6. Usage**

1. **Launch the App:** Open the "Spring of the Hearts (Quran Shorts)" app on your mobile device.
2. **Automatic Playback:** The app will immediately start playing the first Quran Short.
3. **Swipe to Discover:** Swipe vertically (up or down) anywhere on the screen to instantly skip to the next or previous Quran Short in the dynamic, curated stream.
4. **Enjoy:** Immerse yourself in the continuous flow of Quranic recitation with accompanying visuals.

## **7. Future Enhancements**

This starter app lays a strong foundation. Potential future features could include:

-   **Text Overlay Synchronization:** Displaying Arabic Quranic text (and translations) synchronized word-by-word or Ayah-by-Ayah with the audio.
-   **User Profiles & Customization:** Allowing users to select preferred reciters, visual themes, or create personalized short playlists.
-   **Offline Downloads:** Enabling users to download specific "shorts" or collections for offline playback.
-   **Sharing:** Simple, non-intrusive sharing options for individual "shorts."
-   **Feedback Mechanism:** A way for users to provide feedback on "shorts" or report issues.
-   **Continuous Learning Integration:** Optional pop-ups with brief Tafsir (explanation) for an Ayah after it plays.

## **8. License**

This project is open-sourced under the MIT License. See the LICENSE file for more details.
