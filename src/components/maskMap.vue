<template>
    <div class="mask-map" id="mask-map"></div>
</template>

<script>
import L from 'leaflet';

export default {
    name: 'maskMap',

    data() {
        return {
            map: {},
            markers: [],
        };
    },

    computed: {
        currDistrictInfo() {
            return this.$store.getters.currDistrictInfo;
        },

        filteredStores() {
            return this.$store.getters.filteredStores;
        },
    },

    watch: {
        // 切换行政区
        currDistrictInfo(dist) {
            // this.map.panTo() 可以指定地图中心点
            this.map.panTo(new L.LatLng(dist.latitude, dist.longitude));
        },

        filteredStores(stores) {
            this.clearMarkers();

            // 根据药局资讯加上对应 marker
            stores.forEach((element) => this.addMarker(element));
        },
    },

    methods: {
        addMarker(item) {
            // 标记的图示，可以自行更换参数
            const ICON = {
                iconUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-violet.png',
                shadowUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png',
                iconSize: [25, 41],
                iconAnchor: [12, 41],
                popupAnchor: [1, -34],
                shadowSize: [41, 41],                
            };

            // 将标记放在地图上
            const marker = L.marker([item.longitude, item.latitude], ICON)
                            .addTo(this.map)
                            .bindPopup(`<h2 class="popup-name">${item.name}</h2>`);

            marker.markerId = item.id;
            marker.lng = item.longitude;
            marker.lat = item.latitude;

            this.markers.push(marker);
        },

        clearMarkers() {
            this.map.eachLayer((layer) => {
                if (layer instanceof L.Marker) {
                    this.map.removeLayer(layer);
                }
            });

            this.markers.length = 0;
        },

        triggerPopup(markerId) {
            //找出目标标记
            const marker = this.markers.find((d) => d.markerId == markerId);

            // 将地图中心指向目标标记，并开启 Popup
            this.map.flyTo(new L.LatLng(marker.lng, marker.lat), 15);
            marker.openPopup();
        },
    },

    mounted() {
    this.map = L.map('mask-map', {
        center: [25.03, 121.55],
        zoom: 14,
    });
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '<a target="_blank" href="https://www.openstreetmap.org/">© OpenStreetMap 貢獻者</a>',
        maxZoom: 18,
        errorTileUrl:"http://bpic.588ku.com/element_pic/16/12/07/706f7ff4f15725b17ba1d30d384e6468.jpg"
    }).addTo(this.map);
  },
};
</script>
