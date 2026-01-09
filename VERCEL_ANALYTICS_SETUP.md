# Vercel Web Analytics Setup Guide

This document describes the Vercel Web Analytics implementation for the GEM Assist Flask application.

## Overview

Vercel Web Analytics is enabled on this project to track visitor behavior, page views, and performance metrics. The analytics data is automatically collected without requiring any additional npm packages for Flask applications.

## Prerequisites

- A Vercel account with the project deployed
- Admin access to the Vercel dashboard
- The project must be deployed on Vercel

## Setup Status

✅ **Analytics is enabled and configured**

### Configuration Files

1. **vercel.json** - Contains Vercel deployment configuration
   ```json
   {
     "analytics": {
       "enabled": true
     },
     "webAnalytics": {
       "enabled": true
     }
   }
   ```

2. **templates/base.html** - Contains the analytics tracking script
   ```html
   <!-- Vercel Web Analytics -->
   <script>
       window.va = window.va || function () { (window.vaq = window.vaq || []).push(arguments); };
   </script>
   <script defer src="/_vercel/insights/script.js"></script>
   ```

## How It Works

The Vercel Web Analytics script works by:

1. Creating a `window.va` function that queues analytics events
2. Loading the Vercel insights script from `/_vercel/insights/script.js`
3. Automatically tracking page views and user interactions
4. Sending data to Vercel's analytics dashboard

### Automatic Data Collection

Once deployed to Vercel, the following data is automatically collected:

- **Page Views**: Every page visit is tracked
- **Route Information**: The path and query parameters are captured
- **Performance Metrics**: Web Vitals (CLS, FID, LCP) are measured
- **Device Information**: Browser, OS, and device type are recorded
- **Geographic Data**: Visitor location is tracked (based on IP)

## Enabling in Vercel Dashboard

To enable Web Analytics in the Vercel dashboard:

1. Navigate to your [Vercel Dashboard](https://vercel.com/dashboard)
2. Select your project
3. Click the **Analytics** tab
4. Click **Enable** if not already enabled
5. The analytics routes (`/_vercel/insights/*`) will be created after your next deployment

**Note**: Enabling Web Analytics will add new routes scoped at `/_vercel/insights/*` after your next deployment.

## Viewing Analytics Data

Once deployed and users have visited your site:

1. Go to your Vercel dashboard
2. Select your project
3. Click the **Analytics** tab
4. View panels showing:
   - Total visitors
   - Page views
   - Top pages
   - Geographic distribution
   - Device types
   - Performance metrics

### Waiting Period

Analytics data typically begins appearing within a few minutes of deployment. However, for meaningful data, you should wait a few days after deployment to see patterns and trends.

## Custom Events (Pro/Enterprise)

If you're on a Pro or Enterprise plan, you can add custom events to track:

- Button clicks
- Form submissions
- User interactions
- Business metrics
- Purchases or conversions

### Example: Adding a Custom Event

To track a custom event from JavaScript:

```javascript
// Queue a custom event
window.va('event', {
  name: 'button_click',
  metadata: {
    button_name: 'subscribe'
  }
});
```

## Privacy and Compliance

Vercel Web Analytics is designed with privacy in mind:

- **No cookies required**: Analytics work without setting cookies
- **Privacy-friendly**: Uses privacy-first data collection
- **GDPR compliant**: Meets GDPR requirements
- **No PII collected**: Personal information is not stored
- **European data centers**: Data is stored in EU data centers by default

For more details, see [Vercel Analytics Privacy Policy](https://vercel.com/docs/analytics/privacy-policy)

## Next Steps

1. **Deploy to Vercel**: Push your changes to deploy the application
   ```bash
   git push origin main
   ```

2. **Enable in Dashboard**: Log in to Vercel dashboard and enable Web Analytics if not already enabled

3. **Verify Setup**: 
   - Open your site in a browser
   - Open Developer Tools (F12)
   - Go to Network tab
   - Visit different pages
   - You should see requests to `/_vercel/insights/view` and `/_vercel/insights/script.js`

4. **Monitor Dashboard**: Check your analytics dashboard after a few hours to see data

## Filtering Data

Once you have data, you can filter by:

- Date range
- Page path
- Device type
- Operating system
- Browser
- Geographic region

## Troubleshooting

### Analytics Not Showing Data

1. **Verify deployment**: Ensure your app is deployed on Vercel
2. **Check enabled status**: Confirm analytics is enabled in vercel.json
3. **Wait for data**: Give it a few minutes for data to appear
4. **Check Network tab**: Verify `/_vercel/insights/*` requests are being made

### Script Not Loading

If the analytics script isn't loading:

1. Check browser console for errors
2. Verify the Network tab shows successful requests to `/_vercel/insights/script.js`
3. Ensure base.html includes the tracking script
4. Verify `defer` attribute is set on the script tag

## References

- [Vercel Web Analytics Documentation](https://vercel.com/docs/analytics)
- [Vercel Analytics Package](https://vercel.com/docs/analytics/package)
- [Custom Events Guide](https://vercel.com/docs/analytics/custom-events)
- [Filtering Data](https://vercel.com/docs/analytics/filtering)
- [Privacy & Compliance](https://vercel.com/docs/analytics/privacy-policy)

## Support

For issues or questions:

1. Check the [Vercel Analytics FAQ](https://vercel.com/docs/analytics)
2. Review [Vercel Support Documentation](https://vercel.com/help)
3. Contact Vercel support through your dashboard

---

**Last Updated**: January 2026
**Status**: ✅ Active and Configured
