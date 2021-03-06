Basic Control SDK iOS
=====================

This SDK provides access to basic driving controls from a custom iOS application.

DRDouble
========
```
@property (nonatomic, assign) id <DRDoubleDelegate> delegate;
@property (nonatomic, readonly) float poleHeightPercent;
@property (nonatomic, readonly) int kickstandState;
@property (nonatomic, readonly) float batteryPercent;
@property (nonatomic, readonly) BOOL batteryIsFullyCharged;
@property (nonatomic, readonly) NSString *firmwareVersion;

- (void)drive:(DRDriveDirection)forwardBack turn:(float)leftRight; // leftRight is -1.0 to 1.0
- (void)turnByDegrees:(float)theDegrees;
- (void)poleUp;
- (void)poleDown;
- (void)poleStop;
- (void)deployKickstands;
- (void)retractKickstands;
```

DRDoubleDelegate
================
```
- (void)doubleDidConnect:(DRDouble *)theDouble;
- (void)doubleDidDisconnect:(DRDouble *)theDouble;
- (void)doubleStatusDidUpdate:(DRDouble *)theDouble;
- (void)doubleDriveShouldUpdate:(DRDouble *)theDouble;
```

Example Usage (See DRViewController)
====================================
```
- (void)viewDidLoad {
	[super viewDidLoad];
	[DRDouble sharedDouble].delegate = self;
	NSLog(@"SDK Version: %@", kDoubleBasicSDKVersion);
}

- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation {
	return UIInterfaceOrientationIsPortrait(toInterfaceOrientation);
}

#pragma mark - Actions

- (IBAction)poleUp:(id)sender {
	[[DRDouble sharedDouble] poleUp];
}

- (IBAction)poleStop:(id)sender {
	[[DRDouble sharedDouble] poleStop];
}

- (IBAction)poleDown:(id)sender {
	[[DRDouble sharedDouble] poleDown];
}

- (IBAction)kickstandsRetract:(id)sender {
	[[DRDouble sharedDouble] retractKickstands];
}

- (IBAction)kickstandsDeploy:(id)sender {
	[[DRDouble sharedDouble] deployKickstands];
}

#pragma mark - DRDoubleDelegate

- (void)doubleDidConnect:(DRDouble *)theDouble {
	statusLabel.text = @"Connected";
}

- (void)doubleDidDisconnect:(DRDouble *)theDouble {
	statusLabel.text = @"Not Connected";
}

- (void)doubleStatusDidUpdate:(DRDouble *)theDouble {
	poleHeightPercentLabel.text = [NSString stringWithFormat:@"%f", [DRDouble sharedDouble].poleHeightPercent];
	kickstandStateLabel.text = [NSString stringWithFormat:@"%d", [DRDouble sharedDouble].kickstandState];
	batteryPercentLabel.text = [NSString stringWithFormat:@"%f", [DRDouble sharedDouble].batteryPercent];
	batteryIsFullyChargedLabel.text = [NSString stringWithFormat:@"%d", [DRDouble sharedDouble].batteryIsFullyCharged];
	firmwareVersionLabel.text = [DRDouble sharedDouble].firmwareVersion;
}

- (void)doubleDriveShouldUpdate:(DRDouble *)theDouble {
	float drive = (driveForwardButton.highlighted) ? kDRDriveDirectionForward : ((driveBackwardButton.highlighted) ? kDRDriveDirectionBackward : kDRDriveDirectionStop);
	float turn = (driveRightButton.highlighted) ? 1.0 : ((driveLeftButton.highlighted) ? -1.0 : 0.0);
	[theDouble drive:drive turn:turn];
}
```

Note
====
The SDK is experimental and cannot be submitted to the Apple App Store by any third-party developers. If you have any questions about the SDK or its usage, please contact us: info@doublerobotics.com
