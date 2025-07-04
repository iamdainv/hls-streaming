# hls-streaming

## Setup Guide

This project demonstrates how to set up HLS streaming using Node.js for the backend and React.js for the frontend.

### Prerequisites

- Node.js (v14+)
- npm or yarn
- ffmpeg (for video processing)

### Backend (Node.js)

1. **Install dependencies:**

   ```bash
   npm install express fluent-ffmpeg cors
   ```

2. **Set up Express server:**

   - Serve static HLS files (e.g., `.m3u8`, `.ts`).
   - Use `fluent-ffmpeg` to transcode videos to HLS format.

3. **Example server snippet:**

   ```js
   const express = require("express");
   const cors = require("cors");
   const app = express();

   app.use(cors());
   app.use("/hls", express.static("hls"));

   app.listen(8000, () => console.log("Server running on port 8000"));
   ```

4. **Transcode a video to HLS:**
   ```bash
   ffmpeg -i input.mp4 -codec: copy -start_number 0 -hls_time 10 -hls_list_size 0 -f hls hls/output.m3u8
   ```

### Frontend (React.js)

1. **Install a video player library:**

   ```bash
   npm install hls.js
   ```

2. **Sample React component:**

   ```jsx
   import React, { useRef, useEffect } from "react";
   import Hls from "hls.js";

   function VideoPlayer() {
     const videoRef = useRef();

     useEffect(() => {
       if (Hls.isSupported()) {
         const hls = new Hls();
         hls.loadSource("http://localhost:8000/hls/output.m3u8");
         hls.attachMedia(videoRef.current);
       }
     }, []);

     return <video ref={videoRef} controls width="600" />;
   }

   export default VideoPlayer;
   ```

### Notes

- Ensure CORS is enabled on the backend.
- Place your `.m3u8` and `.ts` files in the `hls` directory.
- For production, use a proper media server (e.g., Nginx with the RTMP module).
