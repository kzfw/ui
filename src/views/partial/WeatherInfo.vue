<template>
  <div class="card">
    <div class="card-content">
      <span class="card-title">Current Weather</span>
      <p>The runways listed here are suggestions. If there is a controller online, they may be using different runways than those listed here. Please always check with the controller's ATIS prior to planning your runways.</p>
    </div>
    <div class="table_overflow_wrapper" v-if="numStationsLoaded === Object.keys(stations).length" >
      <table class="striped compact">
        <thead>
          <tr>
            <th>Airport</th>
            <th>Wind</th>
            <th>Conditions</th>
            <th>Landing</th>
            <th>Departing</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="station in stations" :key="station.icao">
            <td><span class="hide-on-med-and-down">{{ station.fullName }} <strong>({{ station.icao }})</strong></span><span class="hide-on-large-only">{{ station.icao }}</span></td>
            <td>{{ formatWind(station) }}</td>
            <td><div class="airport_conditions" v-html="getConditions(station)"></div></td>
            <td>{{ station.getLanding() }}</td>
            <td>{{ station.getDeparting() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="card-content loading" v-else>
      <Spinner />
    </div>
  </div>
</template>

<script>
import { zabApi } from '@/helpers/axios.js';
import parse from 'metar-parser';

export default {
  data() {
    return {
      icao: ['KDFW', 'KDAL', 'KOKC', 'KACT'],
      stations: {
        KDFW: {
          icao: "KDFW",
          fullName: "Dallas-Fort Worth International Airport",
          metar: null, 
          parsedMetar: null,
          configs: {
            landing: {
              N: '31R/35C/35R/36L',
              S: '13R/17L/17C/18R',
              SMid: '17C/18R',
              NMid: '35C/36L',

            }, 
            departing: {
              N: '36R/35L',
              S: '17R/18L',
              SMid: '17R/18L',
              NMid: '35L/36R',
            }
          },
          getLanding: function() {
            if (this.parsedMetar.time.hour >= 4 && this.parsedMetar.time.hour <= 13 && this.parsedMetar.wind.speedKt <= 5) {
                return this.configs.landing.SMid;
            } else if (this.parsedMetar.time.hour >= 4 && this.parsedMetar.time.hour <= 13 && this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.landing.SMid;
            } else if (this.parsedMetar.time.hour >= 4 && this.parsedMetar.time.hour <= 13 && (this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.landing.NMid;
            } else if (this.parsedMetar.wind.speedKt <= 5) {
                return this.configs.landing.S;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.landing.S;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.landing.N;
            } else {
                return this.configs.landing.S;
            }
          },
          getDeparting: function() {
            if (this.parsedMetar.time.hour >= 4 && this.parsedMetar.time.hour <= 13 && this.parsedMetar.wind.speedKt <= 5) {
                return this.configs.departing.SMid;
            } else if (this.parsedMetar.time.hour >= 4 && this.parsedMetar.time.hour <= 13 && this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.departing.SMid;
            } else if (this.parsedMetar.time.hour >= 4 && this.parsedMetar.time.hour <= 13 && (this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.departing.NMid;
            } else if (this.parsedMetar.wind.speedKt <= 5) {
                return this.configs.departing.S;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.departing.S;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.departing.N;
            } else {
                return this.configs.departing.S;
            }
          }
        }, 
        KDAL: {
          icao: "KDAL",
          fullName: "Dallas Love Field Airport",
          metar: null, 
          parsedMetar: null,
          configs: {
            landing: {
              NW: '31L/31R',
              SE: '13L/13R',
            }, 
            departing: {
              NW: '31L/31R',
              SE: '13L/13R',
            }
          },
          getLanding: function() {
            if (this.parsedMetar.wind.speedKt <= 5) {
                return this.configs.landing.SE;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.landing.SE;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.landing.NW;
            } else {
                return this.configs.landing.SE;
            }
          },
          getDeparting: function() {
            if (this.parsedMetar.wind.speedKt <= 5) {
                return this.configs.departing.SE;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.departing.SE;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 6) {
                return this.configs.departing.NW;
            } else {
                return this.configs.departing.SE;
            }
          }
        },
        KOKC: {
          icao: "KOKC",
          fullName: "Will Rogers World Airport",
          metar: null, 
          parsedMetar: null,
          configs: {
            landing: {
              S: '17L/17R',
              N: '35L/35R'
            }, 
            departing: {
              S: '17L/17R',
              N: '35L/35R'
            }
          },
          getLanding: function() {
            if (this.parsedMetar.wind.speedKt <= 10) {
                return this.configs.landing.S;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.landing.S;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.landing.N;
            } else {
                return this.configs.landing.S;
            }
          },
          getDeparting: function() {
            if (this.parsedMetar.wind.speedKt <= 10) {
                return this.configs.departing.S;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.departing.S;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.departing.N;
            } else {
                return this.configs.departing.S;
            }
          }
        }, 
        KACT: {
          icao: "KACT",
          fullName: "Waco Regional Airport",
          metar: null, 
          parsedMetar: null,
          configs: {
            landing: {
              N: '1/32',
              S: '14/19'
            }, 
            departing: {
              N: '1/32',
              S: '14/19'
            }
          },
          getLanding: function() {
            if (this.parsedMetar.wind.speedKt <= 10) {
                return this.configs.landing.S;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.landing.S;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.landing.N;
            } else {
                return this.configs.landing.S;
            }
          },
          getDeparting: function() {
            if (this.parsedMetar.wind.speedKt <= 10) {
                return this.configs.departing.S;
            } else if (this.parsedMetar.wind.direction >= 90 && this.parsedMetar.wind.direction <= 270 && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.departing.S;
            } else if ((this.parsedMetar.wind.direction < 90 || this.parsedMetar.wind.direction > 270) && this.parsedMetar.wind.speedKt >= 11) {
                return this.configs.departing.N;
            } else {
                return this.configs.departing.S;
            }
          }
        },
        /*KRFD: {
          icao: "KRFD",
          fullName: "Chicago Rockford International Airport",
          metar: null, 
          parsedMetar: null,
          configs: {
            landing: {
              SE: '25',
              SW: '25/19',
              NE: '7/1',
            }, 
            departing: {
              SE: '25',
              SW: '25/19',
              NE: '7/1',
            }
          },
          getLanding: function() {
            if(this.parsedMetar.wind.speedKt <= 6) return this.configs.landing.SE;
            else {
              if(this.parsedMetar.wind.direction >= 0 && this.parsedMetar.wind.direction < 130) return this.configs.landing.SW;
              else if(this.parsedMetar.wind.direction >= 130 && this.parsedMetar.wind.direction < 310) return this.configs.landing.NE;
              else return this.configs.landing.SE;
            }
          },
          getDeparting: function() {
            if(this.parsedMetar.wind.speedKt <= 6) return this.configs.departing.SE;
            else {
              if(this.parsedMetar.wind.direction >= 0 && this.parsedMetar.wind.direction < 130) return this.configs.departing.SW;
              else if(this.parsedMetar.wind.direction >= 130 && this.parsedMetar.wind.direction < 310) return this.configs.departing.NE;
              else return this.configs.departing.SE;
            }
          }
        }, 
        KCID: {
          icao: "KCID",
          fullName: "The Eastern Iowa Airport",
          metar: null, 
          parsedMetar: null,
          configs: {
            landing: {
              NE: '9',
              SW: '27',
            }, 
            departing: {
              NE: '9',
              SW: '27',
            }
          },
          getLanding: function() {
            if(this.parsedMetar.wind.speedKt <= 6) return this.configs.landing.NE;
            else {
              if(this.parsedMetar.wind.direction >= 130 && this.parsedMetar.wind.direction < 310) return this.configs.landing.SW;
              else return this.configs.landing.NE;
            }
          },
          getDeparting: function() {
            if(this.parsedMetar.wind.speedKt <= 6) return this.configs.departing.NE;
            else {
              if(this.parsedMetar.wind.direction >= 130 && this.parsedMetar.wind.direction < 310) return this.configs.departing.SW;
              else return this.configs.departing.NE;
            }
          }
        }*/
      },
      numStationsLoaded: 0
    };
  },
  async mounted() {
    await this.getWeatherForAirports();
  },
  methods: {
    async getWeatherForAirports() {
      for (const station of this.icao) {
        const { data } = await zabApi.get(`/ids/stations/${station}`);
        this.stations[station].metar = data.metar;
        this.stations[station].parsedMetar = parse(data.metar);
        this.numStationsLoaded++;

      }
      
    },
    formatWind(station) {
      if(station.parsedMetar.wind.speedKt < 4) return 'Calm';
      else if(!('speedKt' in station.parsedMetar.wind)) return 'Unknown';
      const paddedWind = `0${station.parsedMetar.wind.direction}`.slice(-3);
      return `${paddedWind}@${station.parsedMetar.wind.speedKt}${(station.parsedMetar.wind.gust?`G${station.parsedMetar.wind.gust}`:'')}`;
    },
    getConditions(station) {
      const visibility = station.parsedMetar.visibility.miles;
      const clouds = station.parsedMetar.clouds.filter(cloud => cloud.code === "OVC" || cloud.code === "BKN");
      const verticalVisibility = station.parsedMetar.verticalVisibility || Number.MAX_VALUE;
      let Conditions;

      // Check for LIFR conditions
      if (visibility < 1 || clouds.some(cloud => cloud.altitude < 500) || verticalVisibility < 500) {
        Conditions = `<i class="material-icons weather_icon">wb_cloudy</i>LIFR`;
      }
      // Check for IFR conditions
      else if (visibility >= 1 && visibility < 3 || clouds.some(cloud => cloud.altitude >= 500 && cloud.altitude < 1000) || (verticalVisibility >= 500 && verticalVisibility < 1000)) {
        Conditions = `<i class="material-icons weather_icon">wb_cloudy</i>IFR`;
      }
      // Check for MVFR conditions
      else if (visibility >= 3 && visibility <= 5 || clouds.some(cloud => cloud.altitude >= 1000 && cloud.altitude <= 3000) || (verticalVisibility >= 1000 && verticalVisibility <= 3000)) {
        Conditions = `<i class="material-icons weather_icon">wb_cloudy</i>MVFR`;
      }
      // Otherwise, assume VFR conditions
      else {
        Conditions = `<i class="material-icons weather_icon">wb_sunny</i>VFR`;
      }
      return Conditions;
    }  
  }
};
</script>

<style scoped lang="scss">
.airport_conditions {
  display: inline-flex;
  align-items: center;
  height: 33px;

  &::v-deep(i) {
    margin-right: 5px;
  }
}
.loading {
  text-align: center;
}

.progress {
  max-width: 500px;
  margin: 0 auto;
}

.table_overflow_wrapper {
  width: 100%;
  overflow-x: auto;
}

.weather_icon {
  padding-top: 1em;
}

tbody tr {
  transition: background-color .3s ease;
  &:hover {
    background-color: #eaeaea!important;
  }
}
</style>