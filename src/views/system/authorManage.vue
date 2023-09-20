<template>
    <div>
        <!-- 输入区域 -->
        <div>
            姓名： <input v-model="person.name" type="text">
        </div>
        <div>
            年龄： <input v-model="person.age" type="text">
        </div>
        <div>
            身高： <input v-model="person.height" type="text">
        </div>
        <div>
            <button @click="clickAdd">新增</button>
        </div>

        <!-- 展示区域 -->
        <div>
            <p v-for="(item,index) in list" :key="index">
                <span>性名：{{item.name}} </span>
                <span>年龄：{{item.age}} </span>
                <span>身高：{{item.height}}</span>
                <button @click="clickDel(item.id)">删除</button>
            </p>
        </div>

        <!-- tips -->
        <div v-show="tipsShow" class="tips">！{{msg}}</div>
    </div>
</template>

<script>
export default {
    name: "authorManage",
    data() {
        return {
            person: {
                name: '',
                age: '',
                height: ''
            },
            list: [],
            tipsShow: false,
            msg: ''
        }
    },
    created() {
        this.initDB()
    },
    methods: {
        notify(msg) {
            this.msg = msg;
            this.tipsShow = true;
            setTimeout(() => {
                this.tipsShow = false;
            }, 1000)
        },
        initDB() {
            let request = indexedDB.open('myDB');

            // err
            request.onerror = event => {
                console.log('数据库打开/创建报错', event)
            }

            //如果指定的版本号，大于数据库的实际版本号，就会发生数据库升级事件upgradeneeded。
            //升级后自动触发success
            request.onupgradeneeded = event => {
                let db = event.target.result;
                this.db = db;
                //建表 名为person,主键为id
                let store = db.createObjectStore('person', {
                    keyPath: 'id'
                });
                //新建索引名称、索引所在的属性、配置对象（说明该属性是否允许有重复的值）
                store.createIndex('name', 'name', {
                    unique: false
                });
                store.createIndex('age', 'age', {
                    unique: false
                });
                store.createIndex('height', 'height', {
                    unique: false
                });

                console.log('数据库升级')
            }

            // success
            request.onsuccess = event => {
                this.db = event.target.result;
                console.log('数据库打开/创建成功', event)
                this.getList();
            }
        },
        clickAdd() {
            // 建立读写事务,向对象仓库写入数据记录
            let request = this.db.transaction(['person'], 'readwrite').objectStore('person').add({
                id: new Date().getTime(),
                name: this.person.name,
                age: this.person.age,
                height: this.person.height
            })
            request.onsuccess = event => {
                this.notify('新增成功')
                this.person = {}
                // 获取数据列表
                this.getList()
            };

            request.onerror = event => {
                this.notify('重复数据写入失败')
            }
        },
        read() {
            var transaction = this.db.transaction(['person']);
            let objectStore = transaction.objectStore('person');
            let request = objectStore.get(3);

            request.onerror = function(event) {
                console.log('获取列表失败');
            };

            request.onsuccess = function(event) {
                if (request.result) {
                    console.log('Name: ' + request.result.name);
                    console.log('Age: ' + request.result.age);
                    console.log('Email: ' + request.result.email);
                } else {
                    console.log('未获得数据记录');
                }
            };
        },
        getList() {
            var transaction = this.db.transaction(['person']);
            let objectStore = transaction.objectStore('person');

            let list = [];
            // 遍历数据库
            objectStore.openCursor().onsuccess = event => {
                let cursor = event.target.result;
                console.log(event.target.result,222222222222222)

                if (cursor) {
                    list.push(cursor.value)
                    cursor.continue();
                }else{
                    this.list = list;
                }
            }


        },
        update() {
            let request = this.db.transaction(['person'], 'readwrite').objectStore('person').put({
                id: 1,
                name: '李四',
                age: 35,
                email: '434346463@qq.com',
                test1: 66666,
                test2: 77777
            })
            request.onsuccess = event => {
                console.log('数据更新成功')
            }

            request.onerror = event => {
                console.log('数据更新失败')
            }
        },
        clickDel(id) {
            var request = this.db.transaction(['person'], 'readwrite')
                .objectStore('person')
                .delete(id);

            request.onsuccess = event => {
                this.notify('删除成功')
                this.getList()
            };

            request.onerror = event => {
                this.notify('删除失败')
            }
        }
    }
}
</script>

<style scoped>

</style>
