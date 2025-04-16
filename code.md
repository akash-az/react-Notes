import LoginHeader from "@/containers/loginHeader";
import Header from "@/containers/header";
import Foot from "@/containers/foot";
import Image from "next/image";
import {useRouter} from "next/router";
import {SetStateAction, useEffect, useState} from "react";
import {useRedirectNotLogin} from "@/utils/auth";
import $ from "jquery";
import {decodeFromBase64} from "next/dist/build/webpack/loaders/utils";
import CountdownTimer from "@/utils/CountdownTimer";
import CircleProgressTimer from "@/utils/CircleProgressTimer";
import path from "path";

export default function TaskOverview() {
    const router = useRouter();
    const [taskList, setTaskList] = useState([]);
    const [selectedItem, setSelectedItem] = useState([]);
    
    // ✅ Added for pagination
    const [currentPage, setCurrentPage] = useState(1);
    const itemsPerPage = 6;

    const openModal = (item: []) => setSelectedItem(item);
    const closeModal = () => setSelectedItem([]);

    // ✅ Scroll to top when page changes
    useEffect(() => {
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }, [currentPage]);

    const getTaskOverView = () => {
        $.ajax({
            url: '/api/taskOverview',
            method: "POST",
            success: function (response: { data: string; }) {
                const responseNew: { msg: SetStateAction<string>; status: boolean; data: any; } = decodeFromBase64(response.data);
                if (responseNew.status) {
                    setTaskList(responseNew.data);
                }
            },
            error: function () {}
        });
    }

    useEffect(() => {
        getTaskOverView();
        const interval = setInterval(() => getTaskOverView(), 5000);
        return () => clearInterval(interval);
    }, [router]);

    useEffect(() => {
        $(document).ready(function () {
            $('#search-input').on('input', function () {
                const filterValue = $(this).val().toLowerCase();
                const filterItems = $('[data-emp-name]');
                filterItems.each(function () {
                    const filterName = $(this).attr('data-emp-name').toLowerCase();
                    $(this).toggle(filterName.indexOf(filterValue) > -1);
                });
            });
        });
    }, []);

    const handelDownloadSingleImg = (name: string) => {
        $.ajax({
            url: `/api/assets/loadImg?name=${name}`,
            method: "GET",
            xhrFields: { responseType: 'blob' },
            success: function (data: any) {
                const fileName = path.basename(name);
                const fileExtension = path.extname(name);
                let contentType = {
                    '.jpg': 'image/jpeg',
                    '.jpeg': 'image/jpeg',
                    '.png': 'image/png',
                    '.gif': 'image/gif'
                }[fileExtension.toLowerCase()] || 'application/octet-stream';

                const blob = new Blob([data], { type: contentType });
                const url = window.URL.createObjectURL(blob);
                const $tempLink = $('<a>');
                $tempLink.attr('href', url);
                $tempLink.attr('download', fileName);
                $('body').append($tempLink);
                $tempLink[0].click();
                window.URL.revokeObjectURL(url);
                $tempLink.remove();
            },
            error: function () {}
        });
    }

    const getItem = (item: any) => {
        const picURL = (item.pics && item.pics.length > 0)
            ? `/api/assets/loadImg?name=${item.pics[0]['pic_name']}`
            : "/images/Rectangle 13.png";

        return (
            <div className="row mb-3" data-emp-name={item.empInfo.username}>
                <div className="col-md-12">
                    <div className="card">
                        <div className="card-body">
                            <div className="row align-items-center">
                                <div className="col-md-2">
                                    <Image alt={''} width={136} height={88} src={picURL} className="img-fluid"/>
                                </div>
                                <div className="col-md-4">
                                    <table className="table table-borderless w-100">
                                        <tbody>
                                            <tr><td><strong>User ID</strong></td><td>{item.userInfo.email}</td></tr>
                                            <tr><td><strong>Batch Size</strong></td><td>{item.batchSize}</td></tr>
                                            <tr><td><strong>Batch Name</strong></td><td>{item.number_plate}</td></tr>
                                        </tbody>
                                    </table>
                                </div>
                                <div className="col-md-2 text-center">
                                    <a href="#" onClick={() => openModal(item.pics)} className="btn btnBlue_sm" data-bs-toggle="modal" data-bs-target="#galleryModal">
                                        <i className="far fa-image"></i>&nbsp; View Assets
                                    </a>
                                </div>
                                <CountdownTimer collectionCreatedAt={item.collection_created_at} />
                                <div className="col-md-2">
                                    <CircleProgressTimer timeTask={item.emp_assigned_time} className={"timer_15"} picURL={`${item.empInfo.pic}`} picName={`${item.empInfo.username}`} isRight={false}/>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        );
    };

    // ✅ Pagination logic
    const indexOfLastItem = currentPage * itemsPerPage;
    const indexOfFirstItem = indexOfLastItem - itemsPerPage;
    const currentItems = taskList.slice(indexOfFirstItem, indexOfLastItem);
    const totalPages = Math.ceil(taskList.length / itemsPerPage);

    const paginate = (pageNumber: number) => setCurrentPage(pageNumber);

    const paginationButtons = () => (
        <nav className="d-flex justify-content-center mt-4">
            <ul className="pagination flex-wrap">
                {[...Array(totalPages)].map((_, index) => (
                    <li key={index} className={`page-item ${currentPage === index + 1 ? 'active' : ''}`}>
                        <button onClick={() => paginate(index + 1)} className="page-link">
                            {index + 1}
                        </button>
                    </li>
                ))}
            </ul>
        </nav>
    );

    return (
        <>
            <Header />
            <LoginHeader />
            <section className="marg_1 overview">
                <div className="container-fluid">
                    <div className="row">
                        <div className="col-md-6 ms-auto">
                            <div className="input-group mb-3">
                                <input className="form-control border rounded-pill"
                                       type="search" placeholder="Search by employee name" id="search-input" />
                                <span className="input-group-append ms-2">
                                    <button className="btn btnBlue3" type="button">
                                        <i className="fa fa-search"></i>&nbsp; Search
                                    </button>
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <section className="marg_1 tabDetail">
                <div className="container-fluid">
                    {currentItems.map((item) => getItem(item))}
                    {paginationButtons()}
                </div>
            </section>

            {/* Modal remains unchanged */}
            <div className="modal modal-xl fade" id="galleryModal" tabIndex={-1} aria-labelledby="galleryModalLabel" aria-hidden="true">
                <div className="modal-dialog">
                    <div className="modal-content">
                        <div className="modal-header blueBgL">
                            <div className="modal-title" id="galleryModalLabel">Asset Preview</div>
                            <button type="button" onClick={closeModal} className="btn-close" data-bs-dismiss="modal" aria-label="Close">
                                <i className="far fa-times-circle textBlue" />
                            </button>
                        </div>
                        <div className="modal-body">
                            <div className="container-fluid">
                                {selectedItem.map((item: { id: string, pic_name: string, done_img_url: string }) => (
                                    <div key={item.id} className="row mt-3">
                                        <div className="col-md-6">
                                            <Image width={250} height={250} className="img-fluid" src={`/api/assets/loadImg?name=${item.pic_name}`} alt="original image" />
                                        </div>
                                        <div className="col-md-6">
                                            <Image width={250} height={250} className="img-fluid" src={`/api/assets/loadImg?name=${item.done_img_url}`} alt="processed image" />
                                        </div>
                                    </div>
                                ))}
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <Foot />
        </>
    );
}
