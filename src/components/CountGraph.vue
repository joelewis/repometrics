<template>
<div>
    <div>
        <div id="count-graph"></div>
    </div>
</div>
</template>

<script>
let chartOptions = {
  chart: {
    type: 'column'
  },
  title: {
    text: 'GitHub Project Metrics'
  },
  subtitle: {
    text: 'Source: GitHub.com'
  },
  xAxis: {
    categories: [
        'Forks',
        'Stars',
        'Watches'
    ],
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
                  '<td style="padding:0"><b>{point.y:.0f}</b></td></tr>',
    footerFormat: '</table>',
    shared: true,
    useHTML: true
  },
  plotOptions: {
    column: {
      pointPadding: 0.2,
      borderWidth: 0
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
                data: [project.forks_count, project.stargazers_count, project.watchers_count]
            }
        })

        var canvasElm = this.$el.querySelector('#count-graph');
        chartOptions.series = this.datapoints;
        Highcharts.chart(canvasElm, chartOptions);
    }
}
</script>