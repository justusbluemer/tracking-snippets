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

### Bulk-select (e. g. to delete) variables

Go to the variables overview in Google Tag Manager and execute the following code in your browser's JS console after adding the list of variable names to the `rowCaptions` array.

```
const rowCaptions = [
    // Add list of variable names here
];

function clickCheckboxes() {
    let clickedCount = 0;

    rowCaptions.forEach(caption => {
        // Find the row containing the caption
        const row = Array.from(document.querySelectorAll('tr[gtm-table-row]')).find(row => 
            row.textContent.includes(caption)
        );

        if (row) {
            // Find the checkbox within the row
            const checkbox = row.querySelector('i[role="checkbox"]');
            if (checkbox) {
                checkbox.click();
                clickedCount++;
                console.log(`Clicked checkbox for: ${caption}`);
            } else {
                console.log(`Checkbox not found for: ${caption}`);
            }
        } else {
            console.log(`Row not found for: ${caption}`);
        }
    });

    console.log(`Finished. Clicked ${clickedCount} checkboxes.`);
}

// Run the function
clickCheckboxes();
```
