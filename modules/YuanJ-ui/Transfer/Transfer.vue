<script setup>
    import propsDefine from './extends/props';
    import { 
        useTargetIndex,
        useComputedData,
        useRightListData,
        useCheckedData,
        useDragItem
    } from './extends/hooks';

    import Selector from './components/Selector';

    import ListItem from './components/ListItem'

    const { data, rightTitle } = defineProps(propsDefine);
    
    const options = data.map(({ title }) => title);

    const [ targetIndex, setTargetIndex ] = useTargetIndex(0);

    const [ checkedData, addCheckedData, removeCheckedData ] = useCheckedData();

    const [ dragedItem, setDragedItem ] = useDragItem();

    const [ rightListData, addRightListData, removeRightListData ] = useRightListData([], checkedData);

    const { leftTitle, leftListData } = useComputedData(data, targetIndex, rightListData);

    function setCheckedData(...args) {
        const [ isChecked, leftOrRight, item ] = args;
        isChecked ? addCheckedData(leftOrRight, item) : removeCheckedData(leftOrRight, item.id);
    }


</script>
<template>
    <div>
        <div>
            <selector :data="options" @select-change="setTargetIndex" />
        </div>
        <div class="transfer">
            <div
                class="box left-list"
                @dragover.prevent
                @drop="removeRightListData([dragedItem])"
            >
                <h1 class="list-title">{{ leftTitle }}</h1>
                <div>
                    <list-item 
                        :data="leftListData"
                        left-or-right="left"
                        @checkbox-click="setCheckedData"
                        @drag-item="setDragedItem"
                    />
                </div>
            </div>
            <div class="box btn-group">
                <button @click="removeRightListData(checkedData.right)" :disabled="checkedData.right.length === 0">&lt;</button>
                <button @click="addRightListData(checkedData.left);" :disabled="checkedData.left.length === 0">&gt;</button>
            </div>
            <div
                class="box right-list"
                @dragover.prevent
                @drop="addRightListData([dragedItem])"
            >
                <h1 class="list-title">{{ rightTitle }}</h1>
                <div>
                    <list-item 
                        :data="rightListData"
                        left-or-right="right"
                        @checkbox-click="setCheckedData"
                        @drag-item="setDragedItem"
                    />
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="scss" scoped>
    $main-color: rgb(117, 193, 196);
    .transfer {
        display: flex;
        flex-direction: row;
        width: 600px;
        height: 300px;
        border: 1px solid #ddd;
        .box {
            width: 240px;
            height: 100%;
            .list-title {
                display: flex;
                justify-content: center;
                align-items: center;
                height: 38px;
                font-weight: bold;
                margin: 0;
                color: #666;
                border-bottom: 1px solid #ddd;
                background-color: #efefef;
                font-size: 14px;
            }
            &.left-list {
                .list-item {
                    &.disabled {
                        opacity: .6;
                    }
                }
            }
            &.btn-group {
                display: flex;
                justify-content: center;
                align-items: center;
                width: 120px;
                border-left: 1px solid #ddd;
                border-right: 1px solid #ddd;

                button {
                    text-align: center;
                    border: none;
                    outline: none;
                    height: 38px;
                    width: 48px;
                    color: #fff;
                    background: $main-color;
                    border-radius: 5px;
                    cursor: pointer;
                    transition: all .2s ease;
                    &:disabled {
                        opacity: .6;
                        cursor: default;
                        &:hover {
                            opacity: .6;
                        }
                    }
                    &:hover {
                        opacity: .8;
                    }
                    &:first-child {
                        margin-right: 10px;
                    }
                }
            }
        }
    }
</style>