# Useful tracking snippets
Useful code snippets for web analytics &amp; tracking implementation

## Google Tag Manager

### Log pushed dataLayer events to console

```javascript
(function(){
    const originalPush = window.dataLayer.push
    window.dataLayer.push = (msg) => {
        console.log(msg);
        originalPush(msg);
    }
})();
```

## Google Analytics

### List all GA trackers

```javascript
ga
.getAll()
.forEach(tracker =>
	console.log(
		`name: ${tracker.get("name")} trackingId: ${tracker.get(
			"trackingId"
		)} cookieDomain: ${tracker.get(
			"cookieDomain"
		)} allowLinker: ${tracker.get("allowLinker")}`
	)
);
```
