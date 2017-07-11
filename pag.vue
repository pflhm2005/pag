<template>
    <div class="M-box"></div>
</template>
<script>
    import $ from 'jquery';
    require('./jquery.pagination.js');
    require('./pagination.css');

    export default {
        data: function() {
            return {

            }
        },
        props: ['url', 'pageSize', 'objname', 'keyname', 'totalname'],
        created: function() {
            var v = this;
            $.ajax({
                method: 'post',
                data: {
                    pmId: 123,
                    page: 1,
                    pageSize: v.pageSize
                },
                url: v.url,
                success: function(data) {
                    var u = v.objname ? data.data[v.objname] : data.data;
                    if (data.code === 200) {
                        $('.M-box').pagination({
                            totalData: u[v.totalname],
                            showData: v.pageSize,
                            callback: function(api) {
                                var index = api.getCurrent();
                                $.ajax({
                                    method: 'post',
                                    data: {
                                        pmId: 123,
                                        page: index,
                                        pageSize: v.pageSize
                                    },
                                    url: v.url,
                                    success: function(data) {
                                        u = v.objname ? data.data[v.objname] : data.data;
                                        v.$emit('ajaxRes', u[v.keyname]);
                                    }
                                });
                            }
                        }, function(api) {
                            v.$emit('totalData', data.data);
                            v.$emit('ajaxRes', u[v.keyname]);
                        });
                    }
                }
            })
        }
    }
</script>
<style lang="less">

</style>