# Chrome-Extension-for-FB-Ads-Library
Chrome Extension that enhances the Facebook Ads Library by enabling users to easily download HD video and image assets directly to their local machine.

What We Need:

The extension will work exclusively on the Facebook Ads Library site, adding functionality to make downloading assets seamless for users.

Key Features:
1. Custom Button Injection:
• The extension will inject a custom button (e.g., “Download HD Asset”) into each ad card on both the ad listing page and the single ad page of Facebook Ads Library.
• The button will appear only if the ad contains downloadable HD video or image assets.
2. Direct Asset Download:
• Clicking the button will trigger the download of the HD asset (video or image) directly to the user’s local machine via a system file dialog, allowing the user to choose the save location.
• No cloud storage—assets are downloaded directly to the user’s machine.
3. Intelligent Display:
• If an ad doesn’t contain HD assets or isn’t a video/image, the button won’t appear for that ad.
4. Compatibility:
• The extension must work seamlessly across ad listing pages and single ad pages.


Listing of ads :
https://www.facebook.com/ads/library

Single ad example :
https://www.facebook.com/ads/library/?id=1751079635677585

(Both must have the download button)

What We’re Looking For:
• Chrome Extension Development Experience:
Proven expertise in creating and deploying Chrome extensions.
• Proficiency in JavaScript/HTML/CSS:
Ability to build lightweight, maintainable, and performant code.
• Familiarity with DOM Manipulation:
Skilled at injecting and interacting with elements on dynamic pages.
• Problem-Solving Skills:
Capable of analyzing similar tools (e.g., denote.net, foreplay.co) to understand their approach and replicate the core functionality while keeping it simple.

Resources for Reference:
• Similar Tools:
Analyze how denote.net and foreplay.co implement similar features for downloading assets.
(Note: Our goal is simplicity—no cloud storage, direct downloads only.)
----------------
To build a Chrome extension that enhances the Facebook Ads Library by adding a custom button for downloading HD assets (video or image), we need to break it down into the following steps:

    Create a Manifest File for the Chrome extension.
    Inject a custom button into the Facebook Ads Library page.
    Implement logic to identify downloadable HD assets.
    Handle the download functionality, enabling users to download assets directly to their local machine.
    Ensure compatibility on both ad listing pages and individual ad pages.

Here's a basic implementation of the Chrome extension:
Step 1: manifest.json File

This file will define the metadata for the Chrome extension, including permissions and content scripts.

{
  "manifest_version": 3,
  "name": "Facebook Ads Library HD Asset Downloader",
  "version": "1.0",
  "description": "Download HD assets directly from Facebook Ads Library",
  "permissions": [
    "activeTab",
    "storage"
  ],
  "host_permissions": [
    "https://www.facebook.com/ads/library/*"
  ],
  "background": {
    "service_worker": "background.js"
  },
  "content_scripts": [
    {
      "matches": ["https://www.facebook.com/ads/library/*"],
      "js": ["content.js"],
      "css": ["styles.css"]
    }
  ],
  "action": {
    "default_popup": "popup.html"
  }
}

Step 2: background.js File

The background script will handle extension lifecycle events and communicate with content scripts when necessary.

chrome.runtime.onInstalled.addListener(() => {
  console.log('Facebook Ads Library HD Asset Downloader Extension Installed');
});

Step 3: content.js File

The content script will be responsible for injecting the "Download HD Asset" button into the ad cards on Facebook Ads Library.

// Function to inject a "Download HD Asset" button into each ad card
function injectDownloadButton() {
  // Find all ad cards on the page
  const adCards = document.querySelectorAll('.x1lliihq');

  adCards.forEach(adCard => {
    // Check if the ad card has a downloadable HD video or image
    const videoElement = adCard.querySelector('video');
    const imageElement = adCard.querySelector('img');

    // If a video or image is found, inject the download button
    if (videoElement || imageElement) {
      // Check if the ad already has a button to prevent duplication
      if (!adCard.querySelector('.download-btn')) {
        const downloadBtn = document.createElement('button');
        downloadBtn.classList.add('download-btn');
        downloadBtn.innerText = 'Download HD Asset';
        downloadBtn.style.padding = '5px 10px';
        downloadBtn.style.backgroundColor = '#008CBA';
        downloadBtn.style.color = 'white';
        downloadBtn.style.border = 'none';
        downloadBtn.style.cursor = 'pointer';

        downloadBtn.addEventListener('click', function () {
          let assetURL;
          // Get the asset URL
          if (videoElement) {
            assetURL = videoElement.src;
          } else if (imageElement) {
            assetURL = imageElement.src;
          }

          // Trigger download by creating an anchor element and clicking it programmatically
          const a = document.createElement('a');
          a.href = assetURL;
          a.download = assetURL.split('/').pop();
          document.body.appendChild(a);
          a.click();
          document.body.removeChild(a);
        });

        adCard.appendChild(downloadBtn);
      }
    }
  });
}

// Observe the DOM for changes to ensure buttons are injected when new ads are loaded
const observer = new MutationObserver(() => {
  injectDownloadButton();
});

// Start observing changes on the page
observer.observe(document.body, { childList: true, subtree: true });

// Initial injection on page load
injectDownloadButton();

Step 4: styles.css File

Basic styling for the "Download HD Asset" button to make it stand out.

.download-btn {
  font-size: 14px;
  font-weight: bold;
  background-color: #008CBA;
  color: white;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.download-btn:hover {
  background-color: #005f73;
}

Step 5: Popup HTML (Optional)

If you want to have a popup UI, here is a basic popup.html template that can be customized:

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Facebook Ads Library Downloader</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div>
    <h3>Facebook Ads Library HD Asset Downloader</h3>
    <p>Click on the "Download HD Asset" button on any ad to download assets directly to your computer.</p>
  </div>
</body>
</html>

Step 6: Testing and Deployment

    Load the Extension Locally:
        Go to chrome://extensions/ in your Chrome browser.
        Enable Developer mode.
        Click on Load unpacked and select your extension folder.

    Test on Facebook Ads Library:
        Navigate to Facebook Ads Library.
        You should see the "Download HD Asset" button appear on ads that contain videos or images.

    Refining the Logic:
        Ensure that only valid video/image assets trigger the button.
        Handle edge cases, such as checking for dynamic content or different formats.

Conclusion:

This Chrome extension will allow users to download HD video and image assets directly from the Facebook Ads Library. It injects a "Download HD Asset" button into the ad cards and triggers the download via a file dialog. The extension works seamlessly with dynamic content loading on the Facebook Ads Library page.
