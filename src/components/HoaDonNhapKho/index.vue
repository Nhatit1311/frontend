<template>
    <div class="row">
        <div class="card">
            <div class="card-header">
                Danh Sách Hóa Đơn Nhập Kho
            </div>
            <div class="card-body">
                <div class="table-responesive">
                    <table class="table table-bordered">
                        <thead>
                            <tr class="text-center align-middle">
                                <th>#</th>
                                <th>Mã Hóa Đơn</th>
                                <th>Tổng Tiền</th>
                                <th>Nhà Cung Cấp</th>
                                <th>Ghi Chú</th>
                                <th>Chi Tiết Hóa Đơn</th>
                                <th>Nhân Viên</th>
                            </tr>
                        </thead>
                        <tbody>
                            <template v-for="(value, index) in list_hoa_don">
                                <tr>
                                    <th class="text-center align-middle">{{ index + 1 }}</th>
                                    <td class="text-center align-middle">{{ value.ma_hoa_don }}</td>
                                    <td class="text-end align-middle">{{ formatToVND(value.tong_tien) }}</td>
                                    <td class="align-middle">{{ value.ten_cong_ty}}</td>
                                    <td class="text-center align-middle">
                                        <i class="fa-solid fa-notes-medical fa-2x text-info" v-on:click="Object.assign(detail_hoa_don, value)" data-bs-toggle="modal"
                                        data-bs-target="#ghiChuModal"></i>
                                    </td>
                                    <td class="text-center align-middle">
                                        <i class="fa-solid fa-circle-info fa-2x text-success" v-on:click="getDataChiTietHoaDonNhapKho(value)" data-bs-toggle="modal"
                                        data-bs-target="#chiTietModal"></i>
                                    </td>
                                    <td class="align-middle">{{ value.ho_va_ten}}</td>
                                </tr>
                            </template>
                        </tbody>
                    </table>
                </div>

                <div class="modal fade" id="ghiChuModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h1 class="modal-title fs-5" id="exampleModalLabel">Ghi Chú</h1>
                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                            </div>
                            <div class="modal-body">
                                <textarea v-model="detail_hoa_don.ghi_chu" class="form-control" cols="30" rows="10"></textarea>
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal fade" id="chiTietModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                    <div class="modal-dialog modal-xl">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h1 class="modal-title fs-5" id="exampleModalLabel">Ghi Chú</h1>
                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                            </div>
                            <div class="modal-body">
                                <div class="table-responesive">
                                    <table class="table table-bordered">
                                        <thead>
                                            <tr class="text-center align-middle">
                                                <th>#</th>
                                                <th>Tên Nguyên Liệu</th>
                                                <th>Số Lượng</th>
                                                <th>Đơn Giá</th>
                                                <th>Thành Tiền</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <template v-for="(value, index) in list_chi_tiet_hoa_don">
                                                <tr>
                                                    <th class="text-center align-middle">{{ index + 1 }}</th>
                                                    <td class="align-middle">{{ value.ten_nguyen_lieu }}</td>
                                                    <td class="text-center align-middle">{{ value.so_luong }}</td>
                                                    <td class="text-end align-middle">{{ formatToVND(value.don_gia) }}</td>
                                                    <td class="text-end align-middle">{{ formatToVND(value.thanh_tien) }}</td>
                                                </tr>
                                            </template>
                                            
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { createToaster } from "@meforma/vue-toaster";
const toaster = createToaster({ position: "top-right" });
import baseRequest from '../../core/baseRequest';
export default {
    data() {
        return {
            list_hoa_don                : [],
            list_chi_tiet_hoa_don       : [],
            detail_hoa_don              : {}
        }
    },
    mounted() {
        this.getDataHoaDonNhapKho();
    },
    methods: {
        formatToVND(number) {
            number = parseInt(number);
            return number.toLocaleString('vi-VN', { style: 'currency', currency: 'VND' });
        },
        
        getDataHoaDonNhapKho() {
            baseRequest
                .get('admin/nhap-kho/data-hoa-don-nhap-kho')
                .then((res) => {
                    this.list_hoa_don = res.data.data;
                })
        },

        getDataChiTietHoaDonNhapKho(v) {
            baseRequest
                .post('admin/nhap-kho/data-chi-tiet-hoa-don-nhap-kho', v)
                .then((res) => {
                    this.list_chi_tiet_hoa_don = res.data.data;
                })
        }
    },
}
</script>
<style>
    
</style>
