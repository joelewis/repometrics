<template>
<div>
    <div>
        <div id="commit-graph"></div>
    </div>
</div>
</template>

<script>
let chartOptions = {
  chart: {
    type: 'spline'
  },
  title: {
    text: 'Git Commit Metrics'
  },
  subtitle: {
    text: 'Source: GitHub.com'
  },
  xAxis: {
    categories: Array.from({length: 52}).map((val, i) => 'Week '+(i+1)),
    crosshair: true
  },
  yAxis: {
    min: 0,
    title: {
      text: 'Value'
    }
  },
  tooltip: {
    headerFormat: '<span style="font-size:10px">{point.key}</span><table>',
    pointFormat: '<tr><td style="color:{series.color};padding:0">{series.name}: </td>' +
      '<td style="padding:0"><b>{point.y:.0f} commits</b></td></tr>',
    footerFormat: '</table>',
    shared: true,
    useHTML: true
  },
    plotOptions: {
        spline: {
            lineWidth: 4,
            states: {
                hover: {
                    lineWidth: 5
                }
            },
            marker: {
                enabled: false
            },
        }
    },
    series: []
};

export default {
    props: {
        projects: {
            type: Array,
            default: []
        }
    },

    data: function() {
        return {
            datapoints: [],
        }
    },

    mounted: function() {
        // convert to highcharts compatible format
        this.datapoints = this.projects.map(function(project) {
            return {
                name: project.full_name,
                data: project.commits.map(week => week.total)
            }
        })

        var canvasElm = this.$el.querySelector('#commit-graph');
        chartOptions.series = this.datapoints;
        Highcharts.chart(canvasElm, chartOptions);
    }
}
</script>