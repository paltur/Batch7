<script>
    export default {
        props: ['answer'],

        data(){
            return{
                editing : false,
                body: this.answer.body,
                bodyHtml: this.answer.body_html,
                id: this.answer.id,
                questionId: this.answer.question_id,
                beforeCacheEdit: null
            }
        },
        methods:{
            edit(){
              this.beforeCacheEdit = this.body;
              this.editing = true
            },
            cancel(){
                this.body = this.beforeCacheEdit;
                this.editing = false
            },
            update(){
                axios.patch(this.endpoint,{
                    body:this.body
                }).then(res => {
                    console.log(res);
                    this.editing = false;
                    this.bodyHtml = res.data.body_html;
                    alert(res.data.message);
                }).catch(err => {
                    console.log('Something Went Wrong');
                });
            },
            destroy(){
                if(confirm('Are You Sure')){
                    axios.delete(this.endpoint)
                        .then(res => {
                            $(this.$el).fadeOut(500, () => {
                                console.log(res.data.message);
                            })
                        })
                }
            }
        },
        computed:{
            isInvalid(){
                return this.body.length < 10;
            },
            endpoint(){
                return `/questions/${this.questionId}/answers/${this.id}`;
            }
        }

    }
</script>