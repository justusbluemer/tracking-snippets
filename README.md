# Useful tracking snippets
Useful code snippets for web analytics &amp; tracking implementation

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
