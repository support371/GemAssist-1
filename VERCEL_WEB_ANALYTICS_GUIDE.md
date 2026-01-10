# Getting Started with Vercel Web Analytics

This guide will help you get started with using Vercel Web Analytics on your GEM Assist Flask project, showing you how to enable it, configure your app for Vercel deployment, and view your data in the dashboard.

## Prerequisites

- A Vercel account. If you don't have one, you can [sign up for free](https://vercel.com/signup).
- A Vercel project. If you don't have one, you can [create a new project](https://vercel.com/new).
- The Vercel CLI installed. If you don't have it, you can install it using the following command:

```bash
# Using npm
npm i vercel

# Using yarn
yarn i vercel

# Using pnpm
pnpm i vercel

# Using bun
bun i vercel
```

## Step 1: Enable Web Analytics in Vercel

On the [Vercel dashboard](/dashboard), select your Project and then click the **Analytics** tab and click **Enable** from the dialog.

> **💡 Note:** Enabling Web Analytics will add new routes (scoped at `/_vercel/insights/*`) after your next deployment.

## Step 2: Configure Your Flask Application for Vercel Analytics

For Flask applications, Vercel Web Analytics works automatically once enabled in the dashboard. No additional npm packages are required for Flask backends.

### Verify Configuration Files

1. **vercel.json** - Contains Vercel deployment configuration with analytics enabled

Your `vercel.json` should include:

```json
{
  "buildCommand": "pip install -r requirements.txt",
  "env": {
    "PYTHONUNBUFFERED": "1"
  },
  "framework": "flask",
  "analytics": {
    "enabled": true
  },
  "webAnalytics": {
    "enabled": true
  }
}
```

2. **templates/base.html** - Contains the analytics tracking script

Your base template should include the following script in the `<head>` section:

```html
<!-- Vercel Web Analytics -->
<script>
    window.va = window.va || function () { (window.vaq = window.vaq || []).push(arguments); };
</script>
<script defer src="/_vercel/insights/script.js"></script>
```

## Step 3: Deploy Your App to Vercel

Deploy your app using the Vercel CLI:

```bash
vercel deploy
```

If you haven't already, we also recommend [connecting your project's Git repository](/docs/git#deploying-a-git-repository), which will enable Vercel to deploy your latest commits to main without terminal commands.

Once your app is deployed, it will start tracking visitors and page views.

> **💡 Note:** If everything is set up properly, you should be able to see a Fetch/XHR request in your browser's Network tab from `/_vercel/insights/view` when you visit any page.

## Step 4: View Your Data in the Dashboard

Once your app is deployed, and users have visited your site, you can view your data in the dashboard.

To do so, go to your [dashboard](/dashboard), select your project, and click the **Analytics** tab.

After a few minutes to a few hours of visitors, you'll be able to start exploring your data by viewing and [filtering](/docs/analytics/filtering) the panels.

Users on Pro and Enterprise plans can also add [custom events](/docs/analytics/custom-events) to their data to track user interactions such as button clicks, form submissions, or purchases.

## How Vercel Web Analytics Works

The Vercel Web Analytics system works by:

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

## Tracking Custom Events (Pro/Enterprise Plans)

If you're on a Pro or Enterprise plan, you can add custom events to track user interactions.

### Adding Custom Events via JavaScript

To track a custom event from your frontend:

```javascript
// Queue a custom event
window.va('event', {
  name: 'button_click',
  metadata: {
    button_name: 'subscribe'
  }
});
```

### Example: Track Form Submissions

```html
<form id="contactForm">
  <input type="email" name="email" required>
  <button type="submit">Subscribe</button>
</form>

<script>
  document.getElementById('contactForm').addEventListener('submit', function(e) {
    // Track the event
    window.va('event', {
      name: 'form_submission',
      metadata: {
        form_name: 'contact_form',
        email_provided: true
      }
    });
  });
</script>
```

### Example: Track Button Clicks

```html
<button id="cta-button" class="btn btn-primary">Get Started</button>

<script>
  document.getElementById('cta-button').addEventListener('click', function() {
    window.va('event', {
      name: 'button_click',
      metadata: {
        button_id: 'cta-button',
        section: 'hero'
      }
    });
  });
</script>
```

## Privacy and Compliance

Vercel Web Analytics is designed with privacy in mind:

- **No cookies required**: Analytics work without setting cookies
- **Privacy-friendly**: Uses privacy-first data collection
- **GDPR compliant**: Meets GDPR requirements
- **No PII collected**: Personal information is not stored
- **European data centers**: Data is stored in EU data centers by default

For more details, see [Vercel Analytics Privacy Policy](https://vercel.com/docs/analytics/privacy-policy)

## Filtering and Analyzing Your Data

Once you have data, you can filter by:

- Date range
- Page path
- Device type
- Operating system
- Browser
- Geographic region

## Troubleshooting

### Analytics Not Showing Data

1. **Verify deployment**: Ensure your app is deployed on Vercel using `vercel deploy`
2. **Check enabled status**: Confirm analytics is enabled in Vercel dashboard and vercel.json
3. **Wait for data**: Give it a few minutes for data to appear in the dashboard
4. **Check Network tab**: Verify `/_vercel/insights/*` requests are being made
   - Open browser DevTools (F12)
   - Go to Network tab
   - Visit your site and look for requests to `/_vercel/insights/view` and `/_vercel/insights/script.js`

### Script Not Loading

If the analytics script isn't loading:

1. Check browser console for errors (F12 > Console tab)
2. Verify the Network tab shows successful requests to `/_vercel/insights/script.js`
3. Ensure base.html includes the tracking script in the `<head>` section
4. Verify `defer` attribute is set on the script tag
5. Check that vercel.json has analytics enabled

### No Network Requests to Vercel

1. Verify you're viewing your deployed app at the Vercel URL (not localhost)
2. Confirm deployment completed successfully
3. Check that there are no browser extensions blocking analytics requests
4. Try visiting in an incognito/private window

## Next Steps

Now that you have Vercel Web Analytics set up, you can explore the following topics to learn more:

- [Learn how to use the `@vercel/analytics` package](/docs/analytics/package) (for frontend frameworks)
- [Learn how to set up custom events](/docs/analytics/custom-events)
- [Learn about filtering data](/docs/analytics/filtering)
- [Read about privacy and compliance](/docs/analytics/privacy-policy)
- [Explore pricing](/docs/analytics/limits-and-pricing)
- [Troubleshooting](/docs/analytics/troubleshooting)

## Monitoring Dashboard Features

The Vercel Analytics dashboard provides:

### Core Metrics
- **Visitors**: Unique visitors to your site
- **Page Views**: Total page views
- **Top Pages**: Most visited pages
- **Bounce Rate**: Percentage of single-page sessions
- **Session Duration**: Average time spent on site

### Performance Metrics
- **Web Vitals**: Core Web Vitals (LCP, FID, CLS)
- **Page Load Time**: Average page load time
- **First Contentful Paint (FCP)**
- **Time to Interactive (TTI)**

### Audience Data
- **Geographic Distribution**: Visitors by country/region
- **Device Types**: Desktop, mobile, tablet breakdown
- **Operating Systems**: Windows, macOS, iOS, Android, etc.
- **Browsers**: Chrome, Safari, Firefox, Edge, etc.

## Useful Resources

- [Vercel Web Analytics Documentation](https://vercel.com/docs/analytics)
- [Vercel Analytics Package Documentation](https://vercel.com/docs/analytics/package)
- [Custom Events Guide](https://vercel.com/docs/analytics/custom-events)
- [Filtering Data](https://vercel.com/docs/analytics/filtering)
- [Privacy & Compliance](https://vercel.com/docs/analytics/privacy-policy)
- [Analytics Limits and Pricing](https://vercel.com/docs/analytics/limits-and-pricing)

## Support

For issues or questions:

1. Check the [Vercel Analytics FAQ](https://vercel.com/docs/analytics)
2. Review [Vercel Support Documentation](https://vercel.com/help)
3. Contact Vercel support through your dashboard

---

**Last Updated**: January 2026
**Status**: ✅ Active and Configured for Flask
