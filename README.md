//
//  DemoTests.swift
//  DemoTests
//
//  Created by mattalexander on 4/3/19.
//

import XCTest
@testable import Demo

class DemoTests: XCTestCase {
    
    func testGetFormattedTime() {
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 480, format: "%02d:%02d"), "08:00")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: -1, format: "%02d:%02d"), "")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 1, format: "%02d:%02d"), "00:01")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 1438, format: "%02d:%02d"), "23:58")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 515, format: "%02d:%02d"), "08:35")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 480, format: "%02d:%02d"), "08:00")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 1, format: "%d:%02d"), "0:01")
        XCTAssertEqual(OTTime.shared.getFormattedTime(minutes: 515, format: "%d:%02d"), "8:35")
    }
    
    func testCalculateDifferenceBetweenTwoTimes() {
        XCTAssertEqual(OTTime.shared.getDifferenceBetweenTimes(time1: "0669", time2: "0650"), 0)
        XCTAssertEqual(OTTime.shared.getDifferenceBetweenTimes(time1: "0650", time2: "0650"), 1440)
        XCTAssertEqual(OTTime.shared.getDifferenceBetweenTimes(time1: "0650", time2: "0649"), 1439)
        XCTAssertEqual(OTTime.shared.getDifferenceBetweenTimes(time1: "0650", time2: "1650"), 600)
    }
    
    func testGetTimeDifference() {
        XCTAssertEqual(OTTime.shared.getTimeDifference(timeFrom: "", timeTo: ""), [])
        XCTAssertEqual(OTTime.shared.getTimeDifference(timeFrom: "100", timeTo: "200"), [])
        XCTAssertEqual(OTTime.shared.getTimeDifference(timeFrom: "0600", timeTo: "0600"), ["24","00","24:00"])
        XCTAssertEqual(OTTime.shared.getTimeDifference(timeFrom: "0600", timeTo: "0130"), ["19","30","19:30"])
        XCTAssertEqual(OTTime.shared.getTimeDifference(timeFrom: "0600", timeTo: "0832"), ["02","32","02:32"])
    }
    
    func testGetRoundedTime() {
        var rounded = OTTime.shared.getRoundedTime(time: "2359")
        XCTAssertEqual(rounded, "0000")
        
        rounded = OTTime.shared.getRoundedTime(time: "1233")
        XCTAssertEqual(rounded, "1230")
        
        rounded = OTTime.shared.getRoundedTime(time: "0007")
        XCTAssertEqual(rounded, "0000")

        rounded = OTTime.shared.getRoundedTime(time: "2354")
        XCTAssertEqual(rounded, "0000")

        rounded = OTTime.shared.getRoundedTime(time: "2352")
        XCTAssertEqual(rounded, "2345")

        rounded = OTTime.shared.getRoundedTime(time: "0008")
        XCTAssertEqual(rounded, "0015")

        rounded = OTTime.shared.getRoundedTime(time: "0057")
        XCTAssertEqual(rounded, "0100")
    }
    
    func testIsValid24hrTime() {
        // Fail Cases
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: ""))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: " "))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "0"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "0 0 0 0"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "1"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "2360"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "2400"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "123"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "0060"))
        XCTAssertFalse(OTTime.shared.isValid24hrTime(s: "2999"))

        // Pass Cases
        XCTAssertTrue(OTTime.shared.isValid24hrTime(s: "0000"))
        XCTAssertTrue(OTTime.shared.isValid24hrTime(s: "0001"))
        XCTAssertTrue(OTTime.shared.isValid24hrTime(s: "0013"))
        XCTAssertTrue(OTTime.shared.isValid24hrTime(s: "0113"))
        XCTAssertTrue(OTTime.shared.isValid24hrTime(s: "2359"))
    }
    
}
