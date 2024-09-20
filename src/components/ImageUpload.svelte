<!-- ImageUpload.svelte -->
<script>
    export let image;
    // export let dailyReflection;
</script>

<div class="image-upload and caption entry">
    <h3>Upload a picture and write a caption</h3>
    <input type="file" bind:value={image} accept="image/*">
    <!-- <textarea bind:value={dailyReflection} placeholder="Talk something about the picture"></textarea> -->
</div>

<style>
    input[type="file"] {
        width: 100%;
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 8px;
        margin-top: 0.25rem;
    }
</style>
