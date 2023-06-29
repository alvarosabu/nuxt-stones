<script lang="ts" setup>
import { MeshBasicMaterial } from 'three';

const { scene: model, nodes, materials } = await useGLTF('/nuxt-stones.glb')

// Big Stone 
const stoneBakedTexture = await useTexture(['/RockBaked.png'])
stoneBakedTexture.flipY = false

const stoneBakedMaterial = new MeshBasicMaterial({
  map: stoneBakedTexture
})


nodes.Stone.material = stoneBakedMaterial
nodes.StoneCarved.material = stoneBakedMaterial

// Little stones
const littleStoneBakedTexture = await useTexture(['/LittleRocksBaked.png'])
littleStoneBakedTexture.flipY = false

const littleStoneBakedMaterial = new MeshBasicMaterial({
  map: littleStoneBakedTexture
})

const littleStones = Object.values(nodes).filter(node => node.name.includes('Stone00'))

littleStones.forEach((stone) => {
  stone.material = littleStoneBakedMaterial
})

materials.RockLight.emissiveIntensity = 10

// Animate the emission of the light orbs and logo

const { onLoop } = useRenderLoop()

onLoop(({ elapsed }) => {
  materials.RockLight.emissiveIntensity = Math.sin(elapsed) * 6 + 7
})
</script>

<template>
  <primitive :object="nodes.StoneCarved" />
  <primitive :object="nodes.Stone" />
  <primitive v-for="stone in littleStones" :object="stone" />
  <primitive :object="nodes.Logo" />
  <primitive :object="nodes.Orbs" />
</template>
