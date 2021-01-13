<template>
    <view class="address-edit-page">
        <view class="address-edit-item">
            <view class="address-edit-title">
                Consignee
            </view>
            <view class="address-edit-input-wrapper">
                <input
                    class="address-edit-input"
                    :value="address.name"
                    maxlength="10"
                    placeholder="enter consignee"
                    @input="handleInputName"/>
            </view>
        </view>
        <view class="address-edit-item">
            <view class="address-edit-title">
                Phone
            </view>
            <view class="address-edit-input-wrapper">
                <input
                    class="address-edit-input"
                    :value="address.phone"
                    type="number"
                    maxlength="11"
                    placeholder="enter phone"
                    @input="handleInputPhone"/>
            </view>
        </view>
        <view class="address-edit-item">
            <view class="address-edit-title">
                Area
            </view>
            <view class="address-edit-input-wrapper">
                <picker
                    class="address-edit-picker"
                    mode="multiSelector"
                    :range="cityList"
                    :value="address.area"
                    @columnchange="handlePickerColumnArea">
                    <view class="uni-input">
                        {{address.areaText}}
                    </view>
                </picker>
            </view>
        </view>
        <view class="address-edit-item textarea-edit-item">
            <view class="address-edit-title">
                Detailed
            </view>
            <view class="address-edit-input-wrapper">
                <textarea
                    class="address-edit-textarea"
                    :style="{'height': line * 36 + 76 + 'rpx','padding': line > 1 ? '38rpx 0' : '','line-height': line > 1 ? '36rpx' : '112rpx'}"
                    :value="address.address"
                    maxlength="100"
                    placeholder="enter detailed address"
                    @linechange="handleAddressLine"
                    @input="handleInputAddress"/>
            </view>
        </view>
        <view class="address-edit-item">
            <view class="address-edit-title">
                As Default
            </view>
            <view class="address-edit-switch-wrapper">
                <switch
                    class="address-edit-switch"
                    :checked="address.isDefault"
                    @change="handleSwitchDefault"/>
            </view>
        </view>
        <view
            class="address-delete"
            @click="deleteAddress">
            Delete
        </view>
        <view
            class="address-edit-save"
            @click="saveEdit">
            Save
        </view>
    </view>
</template>

<script>
import Area from '../../common/city.js';
const AreaJson = Area.area;
export default {
    data() {
        return {
            address: {
                name: "",
                phone: "",
                area: [],
                areaText: "",
                address: ""
            },
            cityList: [],
            line: 1
        }
    },
    onLoad(obj) {
        if(obj.id) {
            this.getAddressDetail(obj.id);
        }
        let areaList = [],
            provinces = [],
            cities = [],
            areas = [];
        AreaJson.forEach(item => {
            provinces.push(item.name);
        });
        AreaJson[0].city.forEach(item => {
            cities.push(item.name)
        })
        areas = AreaJson[0].city[0].area;
        areaList.push(provinces)
        areaList.push(cities)
        areaList.push(areas)
        this.cityList = areaList;
    },
    methods: {
        setAreaList(column) {
            let provinceIndex = this.address.area[0],
                cityIndex = this.address.area[1],
                cities = [],
                areas = [];
            switch(column) {
                case 0:
                    AreaJson[provinceIndex].city.forEach(item => {
                        cities.push(item.name)
                    })
                    areas = AreaJson[provinceIndex].city[0].area;
                    this.cityList[1] = cities;
                    this.cityList[2] = areas;
                    this.address.area.splice(1, 1, 0)
                    this.address.area.splice(2, 1, 0)
                    break
                case 1:
                    areas = AreaJson[provinceIndex].city[cityIndex].area;
                    this.cityList[2] = areas;
                    this.address.area.splice(2, 1, 0);
                    break;
            }
            this.address.areaText = this.cityList[0][this.address.area[0]] + '/' + this.cityList[1][this.address.area[1]] + '/' + this.cityList[2][this.address.area[2]];
            this.$forceUpdate()
        },
        getAddressDetail(id) {
            this.address = {
                name: "lanye",
                phone: 13790045784,
                area: [0, 0, 0],
                areaText: "北京市/北京市/东城区",
                address: "一二三四街道 五六七大厦8楼 9012",
                isDefault: false
            }
        },
        handleInputName({ detail }) {
            this.address.name = detail.value;
        },
        handleInputPhone({ detail }) {
            this.address.phone = detail.value;
        },
        handlePickerColumnArea({ detail }) {
            console.log(detail)
            let column = detail.column,
                value = detail.value;
            this.address.area[column] = value;
            this.setAreaList(column);
        },
        handleAddressLine({ detail }) {
            this.line = detail.lineCount + 1;
        },
        handleInputAddress({ detail }) {
            this.address.address = detail.value;
        },
        handleSwitchDefault({ detail }) {
            this.address.isDefault = detail.value;
        },
        deleteAddress() {
            uni.showLoading({
                title: '删除中'
            });
            setTimeout(() => {
                uni.hideLoading();
                uni.showToast({
                    title: "删除成功"
                })
            }, 3000)
        },
        saveEdit() {
            uni.redirectTo({
                url: "/pages/account/address"
            })
        }
    }
}
</script>

<style lang="scss">
page {
    height: 100%;
    input.address-edit-input {
        border: none;
        height: 112rpx;
        line-height: 112rpx;
    }
    textarea.address-edit-textarea {
        width: 100%;
        border: none;
        box-sizing: border-box;
    }
}
</style>

<style lang="scss" scope>
    .address-edit-page {
        height: 100%;
        padding: 0 40rpx 128rpx;
        box-sizing: border-box;
        .address-edit-item {
            padding: 0 12rpx;
            box-sizing: border-box;
            border-bottom: 2rpx solid #f0f0f0;
            height: 114rpx;
            line-height: 112rpx;
            font-size: 28rpx;
            display: flex;
            align-items: center;
            &.textarea-edit-item {
                min-height: 114rpx;
                height: auto;
            }
            .address-edit-title {
                font-weight: bold;
                width: 180rpx;
                color: #333;
            }
            .address-edit-input-wrapper {
                flex-grow: 1;
            }
            .address-edit-picker {
                width: 100%;
                height: 100%;
                border: none;
                line-height: 112rpx;
            }
        }
        .address-delete {
            padding: 0 12rpx;
            box-sizing: border-box;
            height: 112rpx;
            line-height: 112rpx;
            font-size: 28rpx;
            color: #E92C38;
        }
        .address-edit-save {
            position: fixed;
            bottom: 40rpx;
            left: 0;
            margin: 24rpx auto 0;
            width: calc(100% - 30rpx);
            height: 68rpx;
            box-sizing: border-box;
            background-color: #FFAD00;
            color: #fff;
            border-radius: 16rpx;
            line-height: 68rpx;
            text-align: center;
        }
    }
</style>