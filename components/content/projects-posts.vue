<template>
    <section class="not-prose">
        <div class="flex flex-col md:grid md:grid-cols-3 gap-3">
            <article v-for="project in projects" :key="project._path" class="location-listing">
                <NuxtLink class="location-title" :to="project._path">{{ project.title }}</NuxtLink>

                <div class="location-image">
                    <a href="#">
                        <img width="300" height="169" :src="project.image" :alt="project.title"> </a>
                </div>

            </article>
        </div>
    </section>
</template>

<script setup>
const { data: projects } = await useAsyncData(
    'projects-list',
    () => queryContent('/projects')
        .where({ _path: { $ne: '/projects' } })
        .only(['_path', 'title', 'image', 'publishedAt'])
        .sort({ publishedAt: -1 })
        .find()
)
console.log(projects)
</script>
<style scoped>
img {
    width: 100%;
    /* need to overwrite inline dimensions */
    height: auto;
}

/* hover styles */
.location-listing {
    position: relative;
}

.location-image {
    line-height: 0;
    overflow: hidden;
}

.location-image img {
    filter: blur(0px);
    transition: filter 0.3s ease-in;
    transform: scale(1.1);
}

.location-title {
    font-size: 1.5em;
    font-weight: bold;
    text-decoration: none;
    z-index: 1;
    position: absolute;
    height: 100%;
    width: 100%;
    top: 0;
    left: 0;
    opacity: 0;
    transition: opacity .5s;
    background: rgba(90, 0, 10, 0.4);
    color: white;

    /* position the text in t’ middle*/
    display: flex;
    align-items: center;
    justify-content: center;
}

.location-listing:hover .location-title {
    opacity: 1;
}

.location-listing:hover .location-image img {
    filter: blur(2px);
}


/* for touch screen devices */
@media (hover: none) {
    .location-title {
        opacity: 1;
    }

    .location-image img {
        filter: blur(2px);
    }
}
</style>