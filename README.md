## MentorDashboardLayout Component

This layout component dynamically adjusts between showing a sidebar and main content based on screen size. It uses a custom hook `useIsBelowMd` to determine if the screen width is below a medium breakpoint.


```jsx
import React, { useEffect, useState } from 'react'
import { IoNotificationsOutline } from 'react-icons/io5'
import { Outlet } from 'react-router-dom'
import AdminDashboardSidebar from './AdminDashboardSidebar'
import useIsBelowMd from '../../Components/hooks/useIsBelowMd'
import { TbLayoutSidebarFilled } from 'react-icons/tb'
import { BsLayoutSidebarInset } from 'react-icons/bs'

const AdminDashboard = () => {
    const isMobile = useIsBelowMd()
    const [isSidebarOpen, setIsSidebarOpen] = useState(true)

    // Keep sidebar open on larger screens, closed by default on small screens
    useEffect(() => {
        setIsSidebarOpen(!isMobile)
    }, [isMobile])

    const toggleSidebar = () => setIsSidebarOpen(prev => !prev)

    return (
        <div className="max-h-screen min-h-screen flex ">
            {/* Sidebar: visible on desktop; on mobile it's a full-screen panel when open */}
            {(!isMobile || isSidebarOpen) && (
                <section
                    className={
                        isMobile
                            ? 'fixed inset-0 z-50 bg-white w-full h-full shadow-lg' // mobile full-screen sidebar
                            : 'w-[40%] lg:w-[30%] xl:w-[20%]' // desktop sidebar width
                    }
                >
                    <div className=' relative'>
                        <AdminDashboardSidebar onClick={toggleSidebar} />

                        {/* Sidebar toggle icon only on small screens */}
                        {isMobile && (
                            <button
                                onClick={toggleSidebar}
                                aria-label="Toggle sidebar"
                                className="p-2 rounded-md bg-transparent cursor-pointer absolute top-10 right-4"
                            >
                                {isSidebarOpen ? (
                                    <TbLayoutSidebarFilled size={26} />
                                ) : (
                                    <BsLayoutSidebarInset size={26} />
                                )}
                            </button>
                        )}
                    </div>


                </section>
            )}

            {/* Main content area: hide on mobile when sidebar is open */}
            {(!isMobile || !isSidebarOpen) && (
                <section className={`overflow-auto 
                 ${isMobile ? 'w-full' : 'w-[60%] lg:w-[70%] xl:w-[80%]'}`}>
                    <section className="w-full ">
                        {/* Navbar */}
                        <nav className="w-full flex justify-between items-center px-6 py-4 shadow-2xl">
                            <div className="text-primary">
                                <h3 className="font-semibold text-xl">Welcome, Admin</h3>
                                <h5>let's make your work easy</h5>
                            </div>

                            <div className="flex items-center gap-5">
                                <div className="relative p-3 rounded-full bg-[#00A4A61A] cursor-pointer">
                                    <IoNotificationsOutline size={21} />
                                    <div className="absolute top-2 right-2 rounded-full p-1 bg-red-500 h-1 w-1" />
                                </div>

                                <figure className="w-11 h-11 cursor-pointer">
                                    <img
                                        src="https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8dXNlciUyMHByb2ZpbGV8ZW58MHx8MHx8fDA%3D&fm=jpg&q=60&w=3000"
                                        alt=""
                                        className="rounded-full object-cover"
                                    />
                                </figure>

                                {/* Sidebar toggle icon only on small screens */}
                                {isMobile && (
                                    <button
                                        onClick={toggleSidebar}
                                        aria-label="Toggle sidebar"
                                        className="p-2 rounded-md bg-transparent cursor-pointer"
                                    >
                                        {isSidebarOpen ? (
                                            <TbLayoutSidebarFilled size={26} />
                                        ) : (
                                            <BsLayoutSidebarInset size={26} />
                                        )}
                                    </button>
                                )}
                            </div>
                        </nav>
                    </section>

                    <section className='bg-secondary min-h-[calc(100vh-85px)]'>
                        <Outlet />
                    </section>
                </section>
            )}
        </div>
    )
}

export default AdminDashboard
