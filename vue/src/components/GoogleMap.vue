<template>
  <!-- Rending our map -->
  <div class="map">
    <!-- User search bar -->>
    <div class="search-bar">
      <input type="text" id="autocomplete" placeholder="Search for a place" />
      <button id="search-button">Search</button>
    </div>
    <!-- Map-->
    <div id="map"></div>
    <!-- Find my location button -->
    <button id="findLocationButton" @click="findMyLocation">Find My Location</button>
    
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      map: null,
    };
  },

  mounted() {
    console.log("Google Maps loaded successfully"); // This should show up in the console if the maps are loading correctly.
    this.initMap(); // Initialize the map when the component is mounted.
  },

  methods: {
    initMap() {
      this.map = new window.google.maps.Map(document.getElementById("map"), {
        center: { lat: 40.4406, lng: -79.9959 }, // Coordinates for Pittsburgh, PA
        zoom: 10,
        gestureHandling: 'greedy', // This is what allows us to scroll to zoom in and out on the map. This can be set to 'cooperative' to disable the scroll
      });

      // Initialize
      this.completeSearch();
      // this.handleLocationError();
      this.findMyLocation();
      this.addGolfCourseMarker();
      // this.addMarker();
    },

    // Method for finding our current location (geolocation)
    findMyLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const currentPos = {
              lat: position.coords.latitude,
              lng: position.coords.longitude,
            };
            // This sets the map's center to the user's current position
            this.map.setCenter(currentPos);
            this.addMarker(currentPos);
          },
          () => {
            this.handleLocationError(true, this.map.getCenter());
          }
        );
      } else {
        this.handleLocationError(false, this.map.getCenter());
      }
    },


    // I somehow need to add my Array of golf courses in "GolfCourseList.vue" into markers on my map by Lat/Lng. ********
    addGolfCourseMarker() {
      const golfCourses = [
          {
              name: 'Chartiers Country Club',
              address: '601 Baldwin Rd. Pittsburgh, PA 15205-9703',
              latitude: 40.4347404,
              longitude: -80.0929396,
              Website: 'https://chartierscc.com/',
          },
          {
              name: 'Totteridge Golf Club',
              address: '2029 Totteridge Dr. Greensburg, PA 15601',
              latitude: 40.3578107,
              longitude: -79.5065938,
              Website: 'https://www.totteridge.com/',
          },
          {
              name: 'South Hills Country Club',
              address: '4305 Brownsville Rd. Pittsburgh, PA 15236-1998',
              latitude: 40.3609486,
              longitude: -79.9856915,
              Website: 'https://www.southhillscc.org/',
          },
          {
              name: 'Pittsburgh Field Club',
              address: '121 Field Club Rd. Pittsburgh, PA 15238-2224',
              latitude: 40.51304874,
              longitude: -79.89004612,
              Website: 'https://fieldclub.org/',
          },
          {
              name: 'Fox Chapel Golf Club',
              address: '426 Fox Chapel Rd. Pittsburgh, PA 15238-2298',
              latitude: 40.5228106,
              longitude: -79.8773786,
              Website: 'https://www.foxchapelgolfclub.org/',
          },
          {
              name: 'Edgewood Country Club',
              address: '100 Churchill Rd. Pittsburgh, PA 15235-5151',
              latitude: 40.4340465,
              longitude: -79.8334654,
              Website: 'https://eccgolf.com/',
          },
          {
              name: 'St. Clair Country Club',
              address: '2300 Old Washington Rd. Pittsburgh, PA 15241-2428',
              latitude: 40.331386,
              longitude: -80.0780578,
              Website: 'https://www.stclaircc.org/',
          },
          {
              name: 'Lindenwood Golf Club',
              address: '360 Galley Rd. Canonsburg, PA 15317-2391',
              latitude: 40.24803,
              longitude: -80.1339012,
              Website: 'https://www.lindenwoodgolf.com/',
          },
          {
              name: 'Westwood Golf Club',
              address: '825 Commonwealth Ave. West Mifflin, PA 15122-1331',
              latitude: 40.3766558,
              longitude: -79.888783,
              Website: 'https://westwoodlinks.net/',
          },
          {
              name: 'Longue Vue Club',
              address: '400 Longue Vue Dr. Verona, PA 15147-1799',
              latitude: 40.4812932,
              longitude: -79.8586421,
              Website: 'https://longuevue.org/',
          },
          {
              name: 'Grand View Golf Club',
              address: '1000 Clubhouse Dr. Braddock, PA 15104-2800',
              latitude: 40.4952833,
              longitude: -80.2035128,
              Website: 'https://pittsburghgolf.com/',
          },
          {
              name: 'The Club At Nevillewood',
              address: '1000 Nevillewood Dr. Presto, PA 15142-1026',
              latitude: 40.3824025,
              longitude: -80.1244945,
              Website: 'https://www.nevillewood.org/',
          },
          {
              name: 'Oakmont Country Club',
              address: '1233 Hulton Rd. Oakmont, PA 15139-1199',
              latitude: 40.5260435,
              longitude: -79.827508,
              Website: 'https://www.oakmontcc.org/',
          },
          {
              name: 'Moon Golf Club',
              address: '505 McCormick Rd. Moon Township, PA 15108-9359',
              latitude: 40.48253,
              longitude: -80.1887034,
              Website: 'https://www.moongolfclub.com/',
          },
          {
              name: 'Southpointe Golf Club',
              address: '360 SouthPointe Blvd. Canonsburg, PA 15317-8537',
              latitude: 40.2923305,
              longitude: -80.1730783,
              Website: 'https://www.southpointegolfclub.com/',
          },
          {
              name: 'Pittsburgh North Golf Course',
              address: '3800 Bakerstown Rd, Gibsonia, PA 15044',
              latitude: 40.657740,
              longitude: -79.954330,
              Website: 'http://www.pittsburghnorthgolf.com/',
          },
          {
              name: 'Pittsburgh National Golf Club',
              address: '287 Monier Rd, Gibsonia, PA 15044',
              latitude: 40.650600,
              longitude: -79.879740,
              Website: 'https://pittsburghnationalgolfclub.net/',
          },
          {
              name: 'Birdsfoot Golf Course',
              address: '225 Furnace Run Rd, Freeport, PA 16229',
              latitude: 40.745152,
              longitude: -79.665367,
              Website: 'https://www.birdsfoot.com/',
          },
          {
              name: 'Diamond Run Golf Club',
              address: '132 Laurel Oak Dr, Sewickley, PA 15143',
              latitude: 40.559010,
              longitude: -80.102230,
              Website: 'https://www.invitedclubs.com/clubs/diamond-run-golf-club',
          },
          {
              name: 'Latrobe Country Club',
              address: '346 Arnold Palmer Dr, Latrobe, PA 15650',
              latitude: 40.275980,
              longitude: -79.372290,
              Website: 'https://www.latrobecountryclub.com/',
          },
          {
              name: 'Rolling Rock Golf Club',
              address: '167 Club House, Laughlintown, PA 15655',
              latitude: 40.2095341,
              longitude: -79.2122536,
              Website: 'https://rollingrockclub1917.com/',
          },
          {
              name: 'Cranberry Highlands Golf Course',
              address: '5601 Freshcorn Rd, Cranberry Twp, PA 16066',
              latitude: 40.7299311,
              longitude: -80.1383182,
              Website: 'https://www.cranberryhighlands.com/2838/Golf-Course',
          },
      ];

      golfCourses.forEach((course) => {
        const marker = new window.google.maps.Marker({
          position: { lat: course.latitude, lng: course.longitude },
          map: this.map,
          icon: "http://maps.google.com/mapfiles/kml/pal2/icon5.png", // Replace with your custom marker image URL
        });

        // Create an info window for each marker
      const infoWindow = new window.google.maps.InfoWindow({
      content: `
        <div>
          <h4>${course.name}</h4>
          <p>Address: <a href="https://www.google.com/maps/search/?q=${encodeURIComponent(course.address)}" target="_blank">${course.address}</a></p>
          <p>Website: <a href=${course.Website}>${course.Website}</a></p>
        </div>
      `,
    });

    marker.addListener("click", () => {
      infoWindow.open(this.map, marker);

      const closeBtn = infoWindow.getContent().querySelector(".close-button");
      closeBtn.addEventListener("click", () => {
        infoWindow.close();
      })
      });
      });
    },

    // This adds a marker to our current location.
    addMarker(coords) {
      const marker = new window.google.maps.Marker({
        position: coords,
        map: this.map,
        // Find my location marker "green"
        icon: "https://maps.google.com/mapfiles/kml/paddle/grn-circle.png",
      });
      this.map.setZoom(11);
    },



    // If the user does not have location services enabled, an error message will appear.
    handleLocationError(browserHasGeolocation, infoWindow, pos) {
      infoWindow.setPosition(new window.google.maps.LatLng(pos.lat, pos.lng));
      infoWindow.setContent(
        browserHasGeolocation
          ? "Error: The Geolocation service failed."
          : "Error: Your browser doesn't support geolocation."
      );
      infoWindow.open(this.map);
    },

    // This method adds an autocomplete when the user begins typing in the search bar.
    completeSearch() {
      const options = {
        // These are our options for the autocomplete function
        fields: ["place_id", "geometry", "name", "formatted_address"],
        strictBounds: false,
      };
      // This creates an autocomplete object that's associated with the input
      const autocomplete = new window.google.maps.places.Autocomplete(
        document.getElementById("autocomplete"),
        options
      );
      // This will restrict suggestions to our current visible map area
      autocomplete.bindTo("bounds", this.map);
      // This creates a marker when the user searches for a place "Red"
      const auto_marker = new window.google.maps.Marker({
        map: this.map, // Attaches the marker to the map
        anchorPoint: new window.google.maps.Point(0, 50), // Sets anchor point for marker
      });

        /* This section listens for changes when a place is selected from the autocomplete search.
           The marker will hide when a user selects a new place within autocomplete.
        */
      autocomplete.addListener("place_changed", () => {
        auto_marker.setVisible(false);
        const place = autocomplete.getPlace();
        // This stores the place's geo location in 'search_pos'
        this.search_pos = place.geometry.location;
        console.log(this.search_pos);
        if (!place.geometry || !place.geometry.location) {
          window.alert("No details available for input: '" + place.name + "'");
          return;
        }
        if (place.geometry.viewport) {
          this.map.fitBounds(place.geometry.viewport);
        } else {
          this.map.setCenter(place.geometry.location);
          this.map.setZoom(17);
        }
        // Sets the position of the marker to the selected place's location and makes it visible
        auto_marker.setPosition(place.geometry.location);
        auto_marker.setVisible(true);
      });
    },
  },
};
</script>

<!-- MAP STYLING -->
<style scoped>
    #map {
      height: 27rem;
      width: auto;
      border: 10px solid rgb(10, 81, 10);
      margin: 15px;
      padding: 0px;
      position: relative;
      color: black;
    }

    #findLocationButton {
      background-color: #f0f7f0;
      border: solid 5px;
      color: rgb(28, 25, 25);
      padding: 10px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      justify-content: center;
      font-size: 16px;
      margin: 2em;
      cursor: pointer;
      /*This is the position of the button*/
      position: absolute;
      top: 40%;
      left: 45%;
      transform: translate(-50, -50);
    }

    #search-area {
      background-color: white;
      border: none;
      width: auto;
      height: 30px;
      color: rgb(28, 25, 25);
      padding: 1px;
      text-align: left;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 4px 2px;
      image-rendering: auto;
    }

    .search-bar {
      position: relative;
      text-align: left;
      padding-left: 2rem;
    }

    .search-bar input[type="text"] {
      width: auto;
      padding: 10px;
      border: none;
    }

    #search-button {
      height: 40px;
      width: 100px;
    }

    #autocomplete {
      background-color: aliceblue;
      width: 80%;
      height: 40px;
      text-align: middle;
    }

    #start,
    #end {
      background-color: #fff;
      font-family: Roboto;
      font-size: 15px;
      font-weight: 300;
      margin-left: 15px;
      padding: 5px;
      text-overflow: ellipsis;
      width: 200px;
    }

    /* Info window styles */
    .info-window {
      font-size: 16px;
      max-width: 200px;
    }

    .info-window h4 {
      margin: 0;
      font-weight: 700px;
    }

    .info-window p {
      margin: 1px 0;
    }

</style>