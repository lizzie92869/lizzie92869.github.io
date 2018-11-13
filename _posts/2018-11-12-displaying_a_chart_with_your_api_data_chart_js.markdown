---
layout: post
title:      "displaying a chart with your API data (chart.js)"
date:       2018-11-13 00:57:52 +0000
permalink:  displaying_a_chart_with_your_api_data_chart_js
---


I discovered chart.js while working for this entrepreuner who needed to keep track of emotional states for a kids learning application. I would like to share with you the use I have made of this librairy and help you to test it out through this simple application example. 

So basically I had 5 emotions saved through games and I wanted to display different charts to visualize progress through different periods. 

![](https://imgur.com/UhRJM4d.png)	

![](https://imgur.com/ieL4UB0.png)

![](https://imgur.com/WaYjmYt.png)

![](https://imgur.com/9QsxpVb.png)

 This librairy has been a life saver. No doubt I wouldn't have been able to automatically display charts without some help. 
 
 After some setup to build a full-stack Javascript application with the framework Express, I quickly wrote some HTML to display a form that would simulate the data input, saving the date through an API:
 
```
    <form id="form-block" method="POST" action="http://localhost:3000/api/emotional_states">
    Enter a new evaluation

      Date
      <input class="input-date" type="text" required id="datepicker" name="date">

      Anger
      <input class="input-score" type="text" required name="anger"> 
      /10
			
      Happiness</div>
      <input class="input-score" type="text" required name="happiness"> 
      /100

      Sadness
      <input class="input-score" type="text" required name="sadness"> 
      /100
     
		 Frustration
      <input class="input-score" type="text" required name="frustration"> 
      /100

      Empathy
      <input class="input-score" type="text" required name="empathy"> 
      /100
    
    <button type="submit">Save</button>
    </form>
```

and reserved some area for the display of my charts (called canvas):
```

    last evaluation
      <button type="button" id="bar-btn">Bar Chart</button>
      <button type="button" id="horizontal-bar-btn" >Horiz. Bar Chart</button>
    <canvas id="canvas1"></canvas>

   last week
      <button type="button" id="line-btn-week">Line Chart</button>
    <canvas id="canvas2"></canvas>

    last month
      <button type="button" id="line-btn-month">Line Chart</button>
    <canvas id="canvas3"></canvas>

   last year
      <button type="button" id="line-btn-year" >Line Chart</button>
    <canvas id="canvas4"></canvas>
```

You will need to add this librairy to your dependencies: 
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.6.0/Chart.min.js"></script>
```

Then your javascript sheet will need to have a function retrieving the data from your api and displaying them through the chart in the desired canvas. 
I will only show the chart for the last evaluation as the other charts are requiering more work on getting the right data, in the right order and this is not the subject of this post. 

First you will need to have an event listener on the button trigering the chart showing or you may not even need that if you choose to just show the chart when the document load. 

```
  $("#bar-btn").click(function() {
    DisplayLastData('bar');
  });

  $("#horizontal-bar-btn").click(function() {
    DisplayLastData('horizontalBar');
  });
```

then, fetch the data from the APi. In the callback function you will want to filter the data you are looking for.
```
// display the charts for the last evaluation
  function DisplayLastData(chart_type){
    fetch('/api/emotional_states')
    .then((res) => res.json())
    .then((data) => {

      //finding the last data
      const maxValueOfDate = data.map(function(e) { return e.date; }).sort().reverse()[0]

      let sessionEvaluated = data.filter(function (el) {
        return el.date == maxValueOfDate;
      })

      let lastAngerScore = sessionEvaluated[0].anger_score
      let lastHappinessScore = sessionEvaluated[0].happiness_score
      let lastSadnessScore = sessionEvaluated[0].sadness_score
      let lastFrustrationScore = sessionEvaluated[0].frustration_score
      let lastEmpathyScore = sessionEvaluated[0].empathy_score
			
}
```
ti this you want to make sure the previous chart will be destroy before to show the new one as you have 2 ways to trigger the display. 
```
if (myChart) {
            myChart.destroy();
            }
```
You will also need to declare the myChart variable before anything:
```
var myChart;
```
Then, in your callback define the chart configuration and create the instance of the chart with it:
```
          var configChartLastEvaluation = {
            type:`${chart_type}`, // bar, horizontalBar, pie, line, doughnut, radar, polarArea
              data:{
							  // show under each colomn
                labels:['Anger', 'Happiness', 'Sadness', 'Frustration', 'Empathy'],
                datasets:[{
                  label:'Emotion',
                  data:[
                    `${lastAngerScore}`,
                    `${lastHappinessScore}`,
                    `${lastSadnessScore}`,
                    `${lastFrustrationScore}`,
                    `${lastEmpathyScore}`
                  ],

                   //backgroundColor  is the color of each line
                  backgroundColor:[
                    'rgba(255, 99, 132, 0.6)',
                    'rgba(54, 162, 235, 0.6)',
                    'rgba(255, 206, 86, 0.6)',
                    'rgba(75, 192, 192, 0.6)',
                    'rgba(153, 102, 255, 0.6)',
                    'rgba(255, 159, 64, 0.6)',
                    'rgba(255, 99, 132, 0.6)'
                  ],
									//style for each column
                  borderWidth:1,
                  borderColor:'#777',
                  hoverBorderWidth:3,
                  hoverBorderColor:'#000'
                }]
              },
              options:{
							  // keep a 1//1 ratio
                maintainAspectRatio: false,
                title:{
                  display:true,
									//display the date in a human readable format
                  text:`${new Date(sessionEvaluated[0].date).toString().slice(0,15)}`,
                  fontSize:18
                },
                legend:{
                  display:false,
                  position:'right',
                  labels:{
                    fontColor:'#000'
                  }
                },
                layout:{
                  padding:{
                    left:0,
                    right:0,
                    bottom:0,
                    top:0
                  }
                },
                tooltips:{
                  enabled:true
                }
              }
            };
						
	var ctx = document.getElementById("canvas1").getContext("2d");
	var temp = jQuery.extend(true, {}, configChartLastEvaluation);
	
  myChart = new Chart(ctx, temp);   
      
```

All together here is the .js:
```
// display the charts for the last evaluation
  function DisplayLastData(chart_type){
    fetch('/api/emotional_states')
    .then((res) => res.json())
    .then((data) => {

      //finding the last data
      const maxValueOfDate = data.map(function(e) { return e.date; }).sort().reverse()[0]

      let sessionEvaluated = data.filter(function (el) {
        return el.date == maxValueOfDate;
      })

      let lastAngerScore = sessionEvaluated[0].anger_score
      let lastHappinessScore = sessionEvaluated[0].happiness_score
      let lastSadnessScore = sessionEvaluated[0].sadness_score
      let lastFrustrationScore = sessionEvaluated[0].frustration_score
      let lastEmpathyScore = sessionEvaluated[0].empathy_score

          //config variable for the charts of last evaluation
          var configChartLastEvaluation = {
            type:`${chart_type}`, // bar, horizontalBar, pie, line, doughnut, radar, polarArea
              data:{
							// show under each colomn
                labels:['Anger', 'Happiness', 'Sadness', 'Frustration', 'Empathy'],
                datasets:[{
                  label:'Emotion',
                  data:[
                    `${lastAngerScore}`,
                    `${lastHappinessScore}`,
                    `${lastSadnessScore}`,
                    `${lastFrustrationScore}`,
                    `${lastEmpathyScore}`
                  ],

                  //backgroundColor  is the color of each line
                  backgroundColor:[
                    'rgba(255, 99, 132, 0.6)',
                    'rgba(54, 162, 235, 0.6)',
                    'rgba(255, 206, 86, 0.6)',
                    'rgba(75, 192, 192, 0.6)',
                    'rgba(153, 102, 255, 0.6)',
                    'rgba(255, 159, 64, 0.6)',
                    'rgba(255, 99, 132, 0.6)'
                  ],
									//style for each column
                  borderWidth:1,
                  borderColor:'#777',
                  hoverBorderWidth:3,
                  hoverBorderColor:'#000'
                }]
              },
              options:{
							  // keep a 1//1 ratio
                maintainAspectRatio: false,
                title:{
                  display:true,
									//display the date in a human readable format
                  text:`${new Date(sessionEvaluated[0].date).toString().slice(0,15)}`,
                  fontSize:18
                },
                legend:{
                  display:false,
                  position:'right',
                  labels:{
                    fontColor:'#000'
                  }
                },
                layout:{
                  padding:{
                    left:0,
                    right:0,
                    bottom:0,
                    top:0
                  }
                },
                tooltips:{
                  enabled:true
                }
              }
            };

            var ctx = document.getElementById("canvas1").getContext("2d");

            // Remove the old chart and all its event handles
            if (myChart) {
            myChart.destroy();
            }

            // Chart.js modifies the object you pass in. Pass a copy of the object so we can use the original object later
            var temp = jQuery.extend(true, {}, configChartLastEvaluation);
            myChart = new Chart(ctx, temp);   
      });
      
  }
	
 // outside the Ajax query
  var myChart;

  $("#bar-btn").click(function() {
    DisplayLastData('bar');
  });

  $("#horizontal-bar-btn").click(function() {
    DisplayLastData('horizontalBar');
  });

```

Happy coding !
