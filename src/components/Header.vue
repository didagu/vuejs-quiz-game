<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Funny Quiz App</b></b-nav-item>
            <b-nav-item disabled>counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numTotal',
        'numCorrect'
    ]
}
</script>
