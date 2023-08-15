<template>
    <div class='error'>
        <p>{{ error }}</p>
    </div>
</template>

<script>
export default {
    name: 'ErrorMessage',
    props: {
        error: {
            type: String,
            required: true,
        }
    }
}
</script>

<style lang="scss" scoped>
    p {
        color: #ba3838;
    }

    .error {
        text-align: center;
        padding: 20px;
    }
</style>