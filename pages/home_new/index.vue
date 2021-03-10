<scroll-view class="monthDescTab" scroll-x="true" @scroll="scroll" scroll-left="0">
    <view
        v-for="(month, index) in dataArrMonth"
        class="beforeMonth"
        :class="beforeMonthActive == index ? 'active' : ''"
        @click="toMonth(month, index)"
        :key="month"
        v-show="monthTitleShow"
    >
        {{ month }}月
    </view>
</scroll-view>
<style>
.monthDescTab {
    width: 100%;
    /*
    * 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 
    * 只有加入这句话white-space: nowrap; scroll-view组件才能够滑动（此处为横向滑动）
    */
    white-space: nowrap;
    font-size: 30upx;
    height: 50upx;
    line-height: 50upx;
    color: #e8e8e5;
}
</style>