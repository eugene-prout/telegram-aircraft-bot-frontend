<script setup lang="ts">
import { ref } from 'vue'
const props = defineProps<{
    heading: number;
    origin: string;
    destination: string;
    relative_angle: number;
    ground_distance: number;
    aircraft: string;
    aircraft_latitude: number;
    aircraft_longitude: number;
    callsign: string;
    photo?: string | null;
}>();

const show = ref(false)

function convert_bearing_to_eight_point_cardinal(bearing: number) {
    // Ensure the bearing is within the range [0, 360)
    bearing = (bearing % 360 + 360) % 360;

    // Define cardinal directions and corresponding ranges
    const directions = ['N', 'NE', 'E', 'SE', 'S', 'SW', 'W', 'NW'];
    const ranges = [22.5, 67.5, 112.5, 157.5, 202.5, 247.5, 292.5, 337.5];

    for (let i = 0; i < directions.length; i++) {
        if (bearing < ranges[i]) {
            return directions[i];
        }
    }
    return 'N';
}


const flightInfoString = (): string => {
    return `Origin:${props.origin}\nDestination:${props.destination}\nFlying:${convert_bearing_to_eight_point_cardinal(props.heading)}\nLook:${convert_bearing_to_eight_point_cardinal(props.relative_angle)}\nDistance:${Math.round(props.ground_distance)}km`;
};

const dialog = ref(false)

</script>

<template>
    <v-card class="mx-auto" max-width="344">
        <v-img :src="props.photo" height="150px" cover></v-img>

        <v-card-title>
            {{ props.callsign }}
        </v-card-title>

        <v-card-subtitle>
            {{ props.aircraft }}
        </v-card-subtitle>
        <v-divider></v-divider>

        <v-card-text>
            <div>
                <strong>Origin:</strong> {{ props.origin }}<br>
                <strong>Destination:</strong> {{ props.destination }}<br>
                <strong>Flying:</strong> {{ convert_bearing_to_eight_point_cardinal(props.heading) }}<br>
                <strong>Look:</strong> {{ convert_bearing_to_eight_point_cardinal(props.relative_angle) }}<br>
                <strong>Distance:</strong> {{ Math.round(props.ground_distance) }}km
            </div>
        </v-card-text>
        <v-card-actions>
            <v-btn variant="text" color="teal-accent-4" @click="dialog = true">
                Full photo
            </v-btn>
        </v-card-actions>
    </v-card>

    <v-dialog width="800px" v-model="dialog">
        <v-card :title="`${props.aircraft}`">
            <v-img :src="props.photo" cover></v-img>

            <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn text="Close Dialog" @click="dialog = false"></v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>