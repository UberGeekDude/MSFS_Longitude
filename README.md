# MSFS_Longitude
Modification to MSFS2020 Cessna Longitude
Made a few minor changes to the Longitude package provided by dakflu-019 on Fligthsim.to.

https://flightsim.to/file/3655/asobo-cessna-citation-longitude-flight-dynamics-modifications-project-version-1-0

A couple things didn't look right to me according to some pictures. This is not perfect, just minor changes/tweaks for the better of the project.

These changes are based on 1.83a (included)

Parts:
Landing Gear Indication:
1. When first loading the sim, the Landing Gear Indicator is blank.  The code is specific, Up = 0, Down = 1.  However, I dubugged this to see that when launching a flight, the Gear value is 0.9999.... (some number).  Also, it is blank when in transition. I added the text UNSAFE. 
The change in the code below:
						<If>
							<Condition>
								<Equal>
									<Simvar name="GEAR POSITION" unit="number"/>
									<Constant>0</Constant>
								</Equal>
							</Condition>
							<Then>UP</Then>
							<Else>
								<If>
									<Condition>
										<Equal>
											<Simvar name="GEAR POSITION" unit="number"/>
											<Constant>1</Constant>
										</Equal>
									</Condition>
									<Then>DOWN</Then>
									<Else></Else>
								</If>
							</Else>
						</If>
            
Correction:
						<If>
							<Condition>
								<Equal>
									<Simvar name="GEAR POSITION" unit="number"/>
									<Constant>0</Constant>
								</Equal>
							</Condition>
							<Then>UP</Then>
							<Else>
								<If>
									<Condition>
										<Greater>
											<Simvar name="GEAR POSITION" unit="number"/>
											<Constant>0.9</Constant>
										</Greater>
									</Condition>
									<Then>DOWN</Then>
									<Else>UNSAFE</Else>
								</If>
							</Else>
						</If>
            
Trim indicator:
Like IRL, The title for each is is above the graph.  
Original for Aileron:
				<Gauge>
					<Type>Horizontal</Type>							
					<Style>
						<Width>50</Width>
						<ReverseY>True</ReverseY>
					</Style>
					<ID>Turbo_AilTrim</ID>
					<Title>AIL</Title>
					<Unit></Unit>
					<Minimum>-100</Minimum>
					<Maximum>100</Maximum>
					<Value>
						<Simvar name="AILERON TRIM PCT" unit="percent"/>
					</Value>
					<BeginText></BeginText>
					<EndText></EndText>
				</Gauge>
Updated:
				<Text>
					<Center fontsize="18">AILERON</Center>
				</Text>
				<Gauge>
					<Type>Horizontal</Type>							
					<Style>
						<Width>50</Width>
						<ReverseY>True</ReverseY>
					</Style>
					<ID>Turbo_AilTrim</ID>
					<Title></Title>
					<Unit></Unit>
					<Minimum>-100</Minimum>
					<Maximum>100</Maximum>
					<Value>
						<Simvar name="AILERON TRIM PCT" unit="percent"/>
					</Value>
					<BeginText>L</BeginText>
					<EndText>R</EndText>
				</Gauge>

This looks better, to me, but the issue is the Battery Amps id cutoff at the bottom.  The extra text for Aileron and Rudder push the graph down.  I am not sure how to change the column hight for the Aileron and Rudder.  IRL, they are compressed together as well as the engine readouts.

Similar code for Rudder.
