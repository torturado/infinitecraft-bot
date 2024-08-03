## InfiniteCraft bot, auto complete infinitecraft

Just copy and paste this code in the Firefox console (I didn't tried on Chrome):

```
(function simulateClickAndDragAllToSameSpot() {
    let items = Array.from(document.querySelectorAll('div.item'));
    
    if (items.length === 0) {
        console.error('No elements with class "item" found');
        return;
    }

    // Shuffle the items array
    items = items.sort(() => Math.random() - 0.5);

    const endX = window.innerWidth * 0.2;
    const endY = window.innerHeight * 0.2;

    function simulateDragForItem(item, index) {
        const rect = item.getBoundingClientRect();
        let startX = rect.left + rect.width / 2;
        let startY = rect.top + rect.height / 2;

        const mousedownEvent = new MouseEvent('mousedown', {
            bubbles: true, cancelable: true, view: window,
            clientX: startX, clientY: startY
        });
        item.dispatchEvent(mousedownEvent);

        const steps = 10;
        const delay = 10;

        function simulateMouseMove(step) {
            if (step > steps) {
                const mouseupEvent = new MouseEvent('mouseup', {
                    bubbles: true, cancelable: true, view: window,
                    clientX: endX, clientY: endY
                });
                item.dispatchEvent(mouseupEvent);

                // Move to the next item
                if (index < items.length - 1) {
                    setTimeout(() => simulateDragForItem(items[index + 1], index + 1), 200);
                }
                return;
            }

            const progress = step / steps;
            const currentX = startX + (endX - startX) * progress;
            const currentY = startY + (endY - startY) * progress;

            const mousemoveEvent = new MouseEvent('mousemove', {
                bubbles: true, cancelable: true, view: window,
                clientX: currentX, clientY: currentY
            });
            item.dispatchEvent(mousemoveEvent);

            setTimeout(() => simulateMouseMove(step + 1), delay);
        }

        simulateMouseMove(1);
    }

    // Start with the first item in the shuffled array
    simulateDragForItem(items[0], 0);
})();
```

When the script finishes, you can run it again with the new items you've discovered
