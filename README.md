### Live at : 
https://uday5768.github.io/csb-freg65/

### Libraries and modules used :
```
        axios
        chart.js
        plotly.js
```
### Fetching data from given API :
```
const response = await axios.get(
        "https://www.terriblytinytales.com/test.txt"
      );
      const text = response.data;
 ```    
### Parsing the content and finding the frequency of occurrence of each word
```
      const words = text.split(/\s+/);
      const frequencyMap = {};
      words.forEach((word) => {
        if (!frequencyMap[word]) {
          frequencyMap[word] = 1;
        } else {
          frequencyMap[word]++;
        }
      });
```
### Sorting the words based on their frequency of occurrence and selecting the top 20
```
      const sortedWords = Object.keys(frequencyMap).sort(
        (a, b) => frequencyMap[b] - frequencyMap[a]
      );
      const top20Words = sortedWords.slice(0, 20);
```
### Creating the data for the histogram plot
      const plotData = [
        {
          x: top20Words,
          y: top20Words.map((word) => frequencyMap[word]),
          type: "bar"
        }
      ];

### Setting the state with the plot data
```
setData(plotData);
```
### Converting the plot data to CSV format
```
    const csv = `${data[0].x.join(",")}\n${data[0].y.join(",")}`;
```
### Creating a temporary link and triggering the download
```
    const link = document.createElement("a");
    link.href = "data:text/csv;charset=utf-8," + encodeURIComponent(csv);
    link.download = "histogram_data.csv";
    link.click();
```  

### App.js
```
import React, { useState } from "react";
import axios from "axios";
import Plot from "react-plotly.js";

const App = () => {
  const [data, setData] = useState(null);

  const fetchData = async () => {
    try {
      const response = await axios.get(
        "https://www.terriblytinytales.com/test.txt"
      );
      const text = response.data;

      // Parsing the content and finding the frequency of occurrence of each word
      const words = text.split(/\s+/);
      const frequencyMap = {};
      words.forEach((word) => {
        if (!frequencyMap[word]) {
          frequencyMap[word] = 1;
        } else {
          frequencyMap[word]++;
        }
      });

      // Sorting the words based on their frequency of occurrence and selecting the top 20
      const sortedWords = Object.keys(frequencyMap).sort(
        (a, b) => frequencyMap[b] - frequencyMap[a]
      );
      const top20Words = sortedWords.slice(0, 20);

      // Creating the data for the histogram plot
      const plotData = [
        {
          x: top20Words,
          y: top20Words.map((word) => frequencyMap[word]),
          type: "bar"
        }
      ];

      setData(plotData); // Setting the state with the plot data
    } catch (error) {
      console.log(error);
    }
  };

  const downloadCSV = () => {
    if (!data) return;

    // Converting the plot data to CSV format
    const csv = `${data[0].x.join(",")}\n${data[0].y.join(",")}`;

    // Creating a temporary link and triggering the download
    const link = document.createElement("a");
    link.href = "data:text/csv;charset=utf-8," + encodeURIComponent(csv);
    link.download = "histogram_data.csv";
    link.click();
  };

  return (
    <div>
      <button onClick={fetchData}>Submit</button>
      {data && <Plot data={data} />}
      {data && <button onClick={downloadCSV}>Export</button>}
    </div>
  );
};

export default App;
```

