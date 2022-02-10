<template>
    <Lunchbox
        :cameraPosition="[0, 0, 10]"
        :cameraLook="[0, 0, 0]"
        orthographic
        background="indigo"
    >
        <!-- Light -->
        <pointLight :position="[-2, 3, 5]" />

        <!-- LCD -->
        <group :scale="2">
            <!-- Screen -->
            <mesh :scale="6">
                <planeGeometry />
                <meshStandardMaterial color="#cdcbb7" />
            </mesh>

            <!-- Squares -->
            <mesh
                v-for="(v, i) in toggled"
                :key="i"
                :position-x="(i % columns) - columns * 0.5 + 0.5"
                :position-y="
                    -Math.floor(i / columns) +
                    Math.ceil(count / columns) * 0.5 -
                    0.5
                "
                :scale="[0.9, 0.9, 0.001]"
            >
                <boxGeometry />
                <meshStandardMaterial
                    color="#393e40"
                    transparent
                    :opacity="v ? 1 : 0"
                />

                <!-- shadow -->
                <mesh
                    :scale="[1, 1, 0.9]"
                    :position-x="0.025"
                    :position-y="-0.025"
                >
                    <boxGeometry />
                    <meshStandardMaterial color="#b5b49f" />
                </mesh>
            </mesh>
        </group>
    </Lunchbox>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { onBeforeRender } from 'lunchboxjs'

const count = 25
const columns = 5
const toggled = ref(
    new Array(count).fill(undefined).map(() => Math.random() > 0.5)
)

onBeforeRender(() => {
    const now = Date.now()
    toggled.value = toggled.value.map((v, i) => {
        return Math.sin(now * 0.001 + Math.cos(now * 0.0001 * i)) > 0
    })
})
</script>