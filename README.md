## MentorDashboardLayout Component

<details>
  <summary>View Full Code</summary>

```jsx
import React, { useEffect, useState } from 'react';
import { Outlet } from 'react-router-dom';
import useIsBelowMd from '../hooks/useIsBelowMd';
import AdminDashboardNavbar from '../Components/AdminDashboard/AdminDashboardNavbar';
import MentorDashboardSidebar from '../Components/AdminDashboard/MentorDashboardSidebar';

const MentorDashboardLayout = () => {
    const isBelowMd = useIsBelowMd();
    const [viewMode, setViewMode] = useState('sidebar');

    useEffect(() => {
        if (!isBelowMd) {
            setViewMode('both');
        } else {
            setViewMode('sidebar');
        }
    }, [isBelowMd]);

    const toggleView = () => {
        if (isBelowMd) {
            setViewMode((prev) => (prev === 'sidebar' ? 'outlet' : 'sidebar'));
        }
    };

    const handleSidebarItemClick = () => {
        if (isBelowMd) {
            setViewMode('outlet');
        }
    };

    return (
        <div className="px-6 flex min-h-screen bg-gradient-to-br from-white via-[#f7f1ec] to-white">
            {(viewMode === 'sidebar' || viewMode === 'both') && (
                <div className="mt-6">
                    <MentorDashboardSidebar
                        toggleView={toggleView}
                        handleSidebarItemClick={handleSidebarItemClick}
                    />
                </div>
            )}
            <div className="flex-1 p-6">
                {!isBelowMd && (
                    <div>
                        <AdminDashboardNavbar toggleView={toggleView} />
                    </div>
                )}
                {(viewMode === 'outlet' || viewMode === 'both') && (
                    <div
                        style={{ height: 'calc(100vh - 100px)' }}
                        className="overflow-auto"
                    >
                        {isBelowMd && (
                            <div className='mb-5'>
                                <AdminDashboardNavbar toggleView={toggleView} />
                            </div>
                        )}
                        <Outlet />
                    </div>
                )}
            </div>
        </div>
    );
};

export default MentorDashboardLayout;
