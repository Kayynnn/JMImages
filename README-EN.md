# Web Embeds Documentation

This repository contains the HTML, CSS, and JavaScript code for embedding interactive charts, live data summaries, and image carousels into Google Sites. The project is organized into specific pages including Sea Turtle tracking, Waste Management tracking, and historical archives.

## 📂 Folder Structure

```text
JMImages
|   README-EN.md
|   README.md
|
+---2024
|
+---2025
|
+---2026
|
+---Archive2024
|       2025_ActivityGalerry.html
|       2025_BarChart.html
|
+---Archive2025
|       2024_ActivityGalerry.html
|       2024_BarChart.html
|
+---Clean
|
+---SeaTurtle
|       2026_SeaTurtleBarChart.html
|       2026_SeaTurtleGalerry.html
|       2026_SeaTurtleSummary.html
|
\---WasteManagement
        HeroesInAction.html
        TotalWasteCollected.html
        WeightAndIncome.html
```

---

## 📊 1. Live Updates via Google Sheets CSV

The Sea Turtle page utilizes a live data feed from a published Google Sheets CSV to keep the widgets up to date without requiring manual code edits.

**Relevant Files:**

* [2026_SeaTurtleSummary.html](./SeaTurtle/2026_SeaTurtleSummary.html)

* [2026_SeaTurtleBarChart.html](./SeaTurtle/2026_SeaTurtleBarChart.html)


### How the Data Import Works

Both the Summary counters and the Bar Charts use the JavaScript `fetch()` API to download the spreadsheet data via a published CSV link (see [Line 106](./SeaTurtle/2026_SeaTurtleSummary.html), or [Line 85](./SeaTurtle/2026_SeaTurtleBarChart.html)).

* **Summary Counters:** The code splits the downloaded CSV text into rows, targets specifically the third row (index `2`), and extracts the numerical values for Nests, Eggs, and Hatchlings using a `cleanNumber()` helper function that strips away any commas or formatting. These numbers are injected into the `data-turtle-target` attributes to trigger the counting animation.


* **Bar Charts:** The chart script includes a robust `parseCSVLine` function that safely reads rows even if numbers contain commas (e.g., `"1,958"`). It automatically scans the header row (index `0`) for the keywords "Green" and "Hawksbill" to figure out which columns to pull from, providing a failsafe if columns are shifted. It loops through 12 months of data, populates JavaScript arrays (`greenNests`, `hawksNests`, etc.), and feeds those directly into Chart.js.



---

## 🖼️ 2. Updating the Image Carousels

The image carousels display a looping 3-square gallery on desktop (and 1-square on mobile). They rely on images hosted in the `JMImages` GitHub folder.

**Relevant Files:**

* [2024_ActivityGalerry.html](./Archive2025/2024_ActivityGalerry.html)

* [2025_ActivityGalerry.html](./Archive2024/2025_ActivityGalerry.html)

* [2026_SeaTurtleGalerry.html](./SeaTurtle/2026_SeaTurtleGalerry.html)

* [HeroesInAction.html](./WasteManagement/HeroesInAction.html)


### How to Add or Change Images

To change the images, you must edit the HTML directly inside the `<div class="carousel-track" id="imageTrack">` block.

1. Upload your new images to the appropriate folder in the `JMImages` GitHub repository.
2. Get the **Raw** GitHub URL for each image. You can right click on the image in github and copy URL. 
3. In the HTML file, locate the `<div class="carousel-slide">` elements.


4. Replace the `src` attribute with your new direct link. Add or delete these blocks to change the total number of images in the rotation:

```html
<div class="carousel-slide">
    <img src="https://github.com/Kayynnn/JMImages/blob/main/2026/YOUR_NEW_IMAGE.jpg?raw=true" alt="Gallery Image">
</div>

```

---

## 📈 3. Customizing the Archive Bar Charts

Unlike the live 2026 Sea Turtle charts, the archive files contain **static, hardcoded data** that will not change, ensuring historical records are preserved.

**Relevant Files:**

* [2024_BarChart.html](./Archive2025/2024_BarChart.html)

* [2025_BarChart.html](Archive2024/2025_BarChart.html)


### How to Edit Legacy Data

If you need to correct historical data in the archives, you must edit the raw JavaScript arrays inside the `<script>` tag.

1. Open the relevant archive HTML file.
2. Scroll down to the `nestConfig`, `eggConfig`, and `hatchConfig` variables.


3. Locate the `data: [...]` arrays and manually update the numbers. Make sure the numbers align sequentially with the `monthLabels` array.



Example from [2024_BarChart.html](./Archive2025/2024_BarChart.html):

```javascript
const nestConfig = {
  type: 'bar',
  data: {
    labels: monthLabels, // ['Jan', 'Feb', 'Mar', ...]
    datasets: [
      { label: 'Green', data: [0, 2, 7, 21, 50, 68, 63, 63, 36, 20, 1, 0], backgroundColor: colorGreenTurtle, borderRadius: 4 },
      { label: 'Hawksbill', data: [0, 11, 22, 11, 12, 13, 2, 5, 2, 0, 0, 0], backgroundColor: colorHawksbill, borderRadius: 4 }
    ]
  },
  options: commonOptions
};

```

---
