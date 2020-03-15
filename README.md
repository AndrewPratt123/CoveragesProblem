# CoveragesProblem
Submission of the Coverages code test

package com.andrewprattjava;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class Cov {

	private final int eff;
	private final int term;
	
	public Cov(int eff, int term){
	this.eff = eff;
	this.term = term;
	}
	public int getEff() {
		return this.eff;
	}
	public int getTerm() {
		return this.term;
	}
	public static void main(String[] args) {
		List<Cov> coverages = Arrays.asList(new Cov(1,20), new Cov(21,30), new Cov(15,25),
		        new Cov(28,40), new Cov(50, 60), new Cov(61,200));

		// Sort coverage periods by start value (eff)
		//   (streaming into new list since original list is immutable)
		coverages = coverages.stream().sorted(Comparator.comparingInt(Cov::getEff))
		        .collect(Collectors.toList());

		// Iterate coverage periods and find length of longest continuously covered period
		int currEff = -1, currTerm = -1, maxLen = 0;
		for (Cov cov : coverages) {
		    if (cov.getEff() > currTerm + 1) { // Replace current coverage period if gap detected
		        currEff = cov.getEff();
		        currTerm = cov.getTerm();
		    } else if (currTerm < cov.getTerm()) { // Extend current coverage period if needed
		        currTerm = cov.getTerm();
		    }
		    // Update max if current coverage period is longer than any seen so far
		    if (currTerm - currEff >= maxLen)
		        maxLen = currTerm - currEff + 1;
		}

		System.out.println(maxLen); // prints: 151
	}
}
